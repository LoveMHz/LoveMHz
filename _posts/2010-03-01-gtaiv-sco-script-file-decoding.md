---
layout: post
status: publish
published: true
title: GTA:IV SCO Script File Decoding
permalink: /article/gtaiv-sco-script-file-decoding/
author: Dustin
date: '2010-03-01 00:43:00 -0500'
date_gmt: '2010-03-01 00:43:00 -0500'
categories:
- Article
tags: []
thumb: generic.png
---
First off, sorry for the lack of updates.. Been terrible busy with issues going
on in my personal life and also been working on XeBoyAdvance(More details about
  that tomorrow).

I thought I would share with everyone some code I wrote awhile back for decrypting
GTA:IV missions files. Hopefully it will help someone ;)

<!--more-->

```
typedef struct SCO_Header {
        int Identifier;
        int Code_Size;
        int Global_Var_Count;
        int Public_Var_Count;
        int Script_Flags;
        int Signature;
} sco_header;

int DecryptSCO(char * Input) {
    byte GTAKey[32] = {
        0x1A, 0xB5, 0x6F, 0xED, 0x7E, 0xC3, 0xFF, 0x01, 0x22, 0x7B, 0x69, 0x15,
        0x33, 0x97, 0x5D, 0xCE, 0x47, 0xD7, 0x69, 0x65, 0x3F, 0xF7, 0x75, 0x42,
        0x6A, 0x96, 0xCD, 0x6D, 0x53, 0x07, 0x56, 0x5D
    };

    SCO_Header Header;
    char * SCO_Data = NULL;
    char * SCO_Global = NULL;
    char * SCO_Public = NULL;
    int FileSize;

    // Load SCO File  
    ifstream File(Input, ios:: in | ios::binary);
    File.exceptions(ifstream::eofbit | ifstream::failbit | ifstream::badbit);
    if (!File) {
        printf("Cannot open input file.\n");
        return 0;
    }

    // Load the Header Data and get the file size.  
    try {
        File.seekg(0, ios::end);
        FileSize = File.tellg();
        File.seekg(0, ios::beg);
        File.read((char * ) & Header, sizeof(Header));
    } catch (ifstream::failure e) {
        printf("Error reading file. \n");
        return 0;
    }

    // Setup sizes for Global and Public Segment Sizes  
    int Code_Segment_Size = Header.Code_Size & ~0x0F;
    int Global_Segment_Size = (Header.Global_Var_Count * 4) & ~0x0F;
    int Public_Segment_Size = (Header.Public_Var_Count * 4) & ~0x0F;

    // Set sizes to hold the data  
    SCO_Data = new char[Header.Code_Size];
    SCO_Global = new char[Header.Global_Var_Count * 4];
    SCO_Public = new char[Header.Public_Var_Count * 4];

    // Read the segments into each array  
    File.read(SCO_Data, Header.Code_Size);
    if (Global_Segment_Size > 0)
        File.read(SCO_Global, (Header.Global_Var_Count * 4));
    if (Public_Segment_Size > 0)
        File.read(SCO_Public, (Header.Public_Var_Count * 4));

    // Decrpyt each segment  
    if (Code_Segment_Size > 0) {
        for (int i = 0; i < 16; i++) {
            ECB_Mode < AES > ::Decryption ecbDecryption(GTAKey, 32);
            ecbDecryption.ProcessData((byte * ) SCO_Data, (byte * ) SCO_Data, Code_Segment_Size);
        }
    }
    if (Global_Segment_Size > 0) {
        for (int i = 0; i < 16; i++) {
            ECB_Mode < AES > ::Decryption ecbDecryption(GTAKey, 32);
            ecbDecryption.ProcessData((byte * ) SCO_Global, (byte * ) SCO_Global, Global_Segment_Size);
        }
    }
    if (Public_Segment_Size > 0) {
        for (int i = 0; i < 16; i++) {
            ECB_Mode < AES > ::Decryption ecbDecryption(GTAKey, 32);
            ecbDecryption.ProcessData((byte * ) SCO_Public, (byte * ) SCO_Public, Public_Segment_Size);
        }
    }

    // Write everything to a new file  
    ofstream OutFile("out.sco", ios_base::binary);
    if (OutFile.is_open()) {
        // Write File Header to OutFile  
        Header.Identifier = 0xD524353;
        OutFile.write(reinterpret_cast <
            const char * > ( & Header), sizeof(Header));
        // Write Unencrypted data to OutFile  
        OutFile.write(SCO_Data, Header.Code_Size);
        // Write Global and Public data to OutFile  
        OutFile.write((char * ) SCO_Global, Header.Global_Var_Count * 4);
        OutFile.write((char * ) SCO_Public, Header.Public_Var_Count * 4);
        OutFile.close();
    } else {
        printf("Unable to open output file for writing.");
        return 0;
    }

    // Clear out the segment arrays  
    delete[] SCO_Data;
    delete[] SCO_Global;
    delete[] SCO_Public;

    printf("Identifier: 0x%08X \nCode Size: %d \nGlobal Varible Count: %d \nPublic Varible Count: %d \nScript Flags: 0x%08X \nSignature: 0x%08X \nCode_Segment_Size: %d\n",
        Header.Identifier, Header.Code_Size, Header.Global_Var_Count, Header.Public_Var_Count, Header.Script_Flags, Header.Signature, Code_Segment_Size);
    system("PAUSE");
    return 1;
}

int _tmain(int argc, _TCHAR * argv[]) {
    DecryptSCO((char * ) argv[1]);
    return 0;
}  
```
