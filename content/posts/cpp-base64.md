---
title: 利用C++实现的一个简单BASE64编解码器
tags:
  - C++
  - 编程
  - base64
date: 2024-07-08 12:00:32
draft: true
---

test

用位移实现的，不长，直接写在一个文件里了

```C++
#include <iostream>
#include <string>
using namespace std;

char getBase64Char(int x) {
	return "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"[x];
}

char getAsciiChar(int x) {
	return x;
}

int findBase64Char(char x) {
	string str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
	return str.find(x);
}

int findAsciiChar(char x) {
	return x;
}

string bitConversion(string str, const int inputSize, const int outputSize, char (*getChar)(int), int (*findChar)(char)) {
	const int bufferSize = 16;
	
	string result = "";
	unsigned short buffer = 0;
	
	int offset = 0, strLen = str.length();
	
	for(int i=0; i<strLen; ++i) {
		buffer += findChar(str[i]) << (bufferSize-offset-inputSize);
		offset += inputSize;
		
		while(offset >= outputSize) {
			result += getChar(buffer>>(bufferSize-outputSize));
			buffer <<= outputSize;
			offset -= outputSize;
		}
	}
	
	if(offset) {
		result += getChar(buffer>>(bufferSize-outputSize));
	}
	
	return result;
}

string base64Encode(string str) {
	const int strLen = str.length();
	string result = bitConversion(str, 8, 6, getBase64Char, findAsciiChar);
	
	if(strLen % 3 == 1) {
		result += "==";
	}
	else if(strLen % 3 == 2) {
		result += "=";
	}
	return result;
}

string base64Decode(string str) {
	const int strLen = str.length();
	if(str[strLen-2] == '=') {
		str.resize(strLen-2);
	}
	else if(str[strLen-1] == '=') {
		str.resize(strLen-1);
	}
	return bitConversion(str, 6, 8, getAsciiChar, findBase64Char);
}

int main() {
	string str = "";
	
	while(true) {
		printf("Input:");
		getline(cin, str);
		
		str = base64Encode(str);
		printf("Encode: %s\n", str.c_str());
		
		str = base64Decode(str);
		printf("Decode: %s\n", str.c_str());
	}
	
	return 0;
}
```