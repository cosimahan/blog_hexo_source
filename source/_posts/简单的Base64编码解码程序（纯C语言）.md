title: 简单的Base64编码解码程序（纯C语言）
date: 2016-04-19 20:50:24
tags:
---

先声明一下这其中编码解码的算法部分不是我手打的，是参考现有的程序。
该代码编译后可直接运行，亦可带参运行，程序内有提示。
有时间我会把它做成PHP版，那就更方便了。

> main.c
```
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
const char base[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
char* base64_encode(const char* data, int data_len);
char* base64_decode(const char* data, int data_len);
void do_decode(char*);
void do_encode(char*);
static char find_pos(char ch);
const int MAXIN = 1001;
int main(int argc, char* argv[])
{

	char e_d = 'a';
	char t[MAXIN] = "";
	//printf("%d@%s@%s", argc, argv[0], argv[1]);
	if (argc > 1) {
		if ((argc == 2 || argc == 3) && (argv[1][1] == 0) && (argv[1][0] == 'e' || argv[1][0] == 'd')) {

			e_d = argv[1][0];
			if (e_d == 'e') {
				if (argc == 2) {
					printf("Max input length is %d\n", MAXIN - 1);
					scanf("%s", t);
					do_encode(t);
				}
				else do_encode(argv[2]);
			}
			else if (e_d == 'd') {
				if (argc == 2) {
					printf("Max input length is %d\n", MAXIN - 1);
					scanf("%s", t);
					do_decode(t);
				}
				else do_decode(argv[2]);
			}
		}
		else printf(" Usage: %s Commands [String] \nCommands:\n\te - encode\n\td - decode\n",argv[0]);
	}
	else {
		printf("Base64 encode/decode(e/d) ?\n");
		char input_ed[111] = "";
		while ((scanf("%s", input_ed)), e_d = input_ed[0], (e_d != 'e' && e_d != 'd' || input_ed[1] != 0))printf("Input Eror\n");


		printf("Max input length is %d, enter CTRL+C to exit.\n", MAXIN - 1);

		while (printf("\nPlease input: ")) {

			scanf("%s", t);


			if (e_d == 'e') {
				do_encode(t);
			}
			else if (e_d == 'd') {
				do_decode(t);
			}
			else {
				printf("ERROR\n");
				break;
			}
		}
	}

	return 0;
}
/* */
void do_encode(char* t) {

	int j = strlen(t);

	char *enc = base64_encode(t, j);
	int len = strlen(enc);
	char *dec = base64_decode(enc, len);
	printf("\noriginal: %s\n", t);
	printf("\nencoded : %s\n", enc);
	printf("\ndecoded : %s\n", dec);
	free(enc);
	free(dec);

}
void do_decode(char* t) {
	//char t[MAXIN] = "";
	//scanf("%s", t);
	//int i = 0;
	int j = strlen(t);

	char *dec = base64_decode(t, j);
	int len = strlen(dec);
	char *enc = base64_encode(dec, len);
	printf("\noriginal: %s\n", t);
	printf("\ndecoded : %s\n", dec);
	printf("\nencoded : %s\n", enc);
	free(enc);
	free(dec);
}
char *base64_encode(const char* data, int data_len)
{
	//int data_len = strlen(data); 
	int prepare = 0;
	int ret_len;
	int temp = 0;
	char *ret = NULL;
	char *f = NULL;
	int tmp = 0;
	char changed[4];
	int i = 0;
	ret_len = data_len / 3;
	temp = data_len % 3;
	if (temp > 0)
	{
		ret_len += 1;
	}
	ret_len = ret_len * 4 + 1;
	ret = (char *)malloc(ret_len);

	if (ret == NULL)
	{
		printf("No enough memory.\n");
		exit(0);
	}
	memset(ret, 0, ret_len);
	f = ret;
	while (tmp < data_len)
	{
		temp = 0;
		prepare = 0;
		memset(changed, '\0', 4);
		while (temp < 3)
		{
			//printf("tmp = %d\n", tmp); 
			if (tmp >= data_len)
			{
				break;
			}
			prepare = ((prepare << 8) | (data[tmp] & 0xFF));
			tmp++;
			temp++;
		}
		prepare = (prepare << ((3 - temp) * 8));
		//printf("before for : temp = %d, prepare = %d\n", temp, prepare); 
		for (i = 0; i < 4; i++)
		{
			if (temp < i)
			{
				changed[i] = 0x40;
			}
			else
			{
				changed[i] = (prepare >> ((3 - i) * 6)) & 0x3F;
			}
			*f = base[changed[i]];
			//printf("%.2X", changed[i]); 
			f++;
		}
	}
	*f = '\0';

	return ret;

}
/* */
static char find_pos(char ch)
{
	char *ptr = (char*)strrchr(base, ch);//the last position (the only) in base[] 
	return (ptr - base);
}
/* */
char *base64_decode(const char *data, int data_len)
{
	int ret_len = (data_len / 4) * 3;
	int equal_count = 0;
	char *ret = NULL;
	char *f = NULL;
	int tmp = 0;
	int temp = 0;
	char need[3];
	int prepare = 0;
	int i = 0;
	if (*(data + data_len - 1) == '=')
	{
		equal_count += 1;
	}
	if (*(data + data_len - 2) == '=')
	{
		equal_count += 1;
	}
	if (*(data + data_len - 3) == '=')
	{//seems impossible 
		equal_count += 1;
	}
	switch (equal_count)
	{
	case 0:
		ret_len += 4;//3 + 1 [1 for NULL] 
		break;
	case 1:
		ret_len += 4;//Ceil((6*3)/8)+1 
		break;
	case 2:
		ret_len += 3;//Ceil((6*2)/8)+1 
		break;
	case 3:
		ret_len += 2;//Ceil((6*1)/8)+1 
		break;
	}
	ret = (char *)malloc(ret_len);
	if (ret == NULL)
	{
		printf("No enough memory.\n");
		exit(0);
	}
	memset(ret, 0, ret_len);
	f = ret;
	while (tmp < (data_len - equal_count))
	{
		temp = 0;
		prepare = 0;
		memset(need, 0, 3);
		while (temp < 4)
		{
			if (tmp >= (data_len - equal_count))
			{
				break;
			}
			prepare = (prepare << 6) | (find_pos(data[tmp]));
			temp++;
			tmp++;
		}
		prepare = prepare << ((4 - temp) * 6);
		for (i = 0; i<3; i++)
		{
			if (i == temp)
			{
				break;
			}
			*f = (char)((prepare >> ((2 - i) * 8)) & 0xFF);
			f++;
		}
	}
	*f = '\0';
	return ret;
}

```
