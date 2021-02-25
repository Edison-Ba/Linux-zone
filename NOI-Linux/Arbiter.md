```c
#include <stdlib.h>
#include <stdio.h>
#include <math.h>
#include <string.h>

#define EVAL_OUTPUT_FILE "/tmp/_eval.score"

FILE *fpout, *fpans, *fpeval , *fpin;
/* Output message and exit */
void output (char *s, int d)//输出成绩 
{
	if (fpeval) {
		fprintf (fpeval, "%s\n%d\n", s, d);
		fclose (fpeval);
	}
	exit(d != 0);
}

/* Open files and check */
void open_files(char *in,char *out, char *ans, char *eval_output)
{
	if ((fpeval = fopen (eval_output, "w")) == NULL) {
		fprintf (stderr, "Can not open %s!\n", EVAL_OUTPUT_FILE);//这是成绩输出文件 
		output ("Can not open eval record file!", 0);
	}
	
	if ((fpin = fopen (in, "r")) == NULL) {
		fprintf (stderr, "Can not open %s!\n", in);//这是std输入文件 
		output ("Can not open standard input!", 0);
	}
	
	if ((fpout = fopen (out, "r")) == NULL) {//这是选手输出文件 
		fprintf (stderr, "Can not open %s!\n", out);
		output ("Can not open player's output file!", 0);
	}

	if ((fpans = fopen (ans, "r")) == NULL) {
		fprintf (stderr, "Can not open %s!\n", ans);//这是std输出文件 
		output ("Can not open standard answer!", 0);
	}
}
char s[100001];
int main (int argc, char *argv[])
{
	long d1 = -1, d2 = -1;
	int error = 0;
		
	if (argc != 4) {
		fprintf (stderr, "Usage: mason_e <in> <out> <ans>\n");
		output ("Invalid Call!", 0);
	}

	open_files ( argv[1] , argv[2], argv[3], EVAL_OUTPUT_FILE);

/* 

在下面写代码，
fpout是选手的输出,fpans是std的输出(用fscanf),spin是输入内容
(fpeval是结果输出文件)
feof(filename)是判断后面是否为eof
output (char* f, int k);输出内容f，成绩k,非0为ac 
 
*/	
//	fscanf(fpans,"%s",s);
//	if(strcmp(s,"orz")==0)output("sto",10);
//	else if(strcmp(s,"2")==0)output("Correct!",10);
//	output("???",0);

	return 0;
}
```

另外：

1. 选手名单导入(.csv文件)

格式每个人一行：

```
考号,名字
```

例如

```
SD-0001,a
SD-0002,b
SD-0003,c
SD-0004,d
```

2. 测试点在evaldata文件夹，全放目录里没有子文件夹，是.in和.ans，没有.out。

3. 选手程序在players文件夹下(只写考号)。

4. spj放在filter文件夹下，要编译。

5. 结果在result文件夹下。


读取选手名单并改文件名：
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
char s1[1001],s2[1001],R[2050];
int main(){
    freopen("player.csv","r",stdin);
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(;;){
        s1[0]=getchar();
        if(s1[0]==EOF)break;
        int j;
        for(j=1;s1[j-1]!=',';j++)s1[j]=getchar();
        s1[j-1]='\0';
        s2[0]=getchar();
        if(s2[0]==EOF)break;
        for(j=1;s2[j-1]!=','&&s2[j-1]!='\r'&&s2[j-1]!='\n';j++)s2[j]=getchar();
        int flag=1;
        if(s2[j-1]=='\n')flag=0;
        char Ch=s2[j-1];
		s2[j-1]='\0';
        
        if(flag){
			Ch=getchar();
        	while(Ch!='\n'&&Ch!=EOF)Ch=getchar();	
		}
        sprintf(R,judge==0?"rename %s%s %s":"mv %s%s %s",s1,s2,s1);
        system(R);
        printf(R);
        putchar('\n');
        if(Ch==EOF)break;
    }
    return 0;
}
		
```
.out改.ans

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int Zushu=1;
char Name[]="aplusb";
char s1[1001],s2[1001],R[2050];
int main(){
	int judge=system("cls");            // 0 为 Windows，>0 为 Linux
	for(int i=1;i<=Zushu;i++){
		sprintf(R,judge==0?"rename %s%d.out %s%d.ans":"mv %s%d.out %s%d.ans",Name,i,Name,i);
		system(R);
		printf(R);putchar('\n');
	}
	return 0;
}

```
去'\r'(在Linux下运行)

```
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int Zushu=1;
char Name[]="aplusb";
char s1[1001],s2[1001],R[2050];
int main(){
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(int i=1;i<=Zushu;i++){
        sprintf(R,"cp %s%d.in MEIYONG.txt",Name,i);
        system(R);
        printf(R);
        putchar('\n');
        sprintf(R,"%s%d.in",Name,i);
        FILE *fin=fopen("MEIYONG.txt","r"),*fout=fopen(R,"w");
        char ch;
        while((ch=fgetc(fin))!=EOF)if(ch!='\r')fputc(ch,fout);
        fclose(fin);fclose(fout);
        sprintf(R,"rm MEIYONG.txt");
        system(R);
        printf(R);
        putchar('\n');
        
        sprintf(R,"cp %s%d.ans MEIYONG.txt",Name,i);
        system(R);
        printf(R);
        putchar('\n');
        sprintf(R,"%s%d.ans",Name,i);
        fin=fopen("MEIYONG.txt","r"),fout=fopen(R,"w");
        //char ch;
        while((ch=fgetc(fin))!=EOF)if(ch!='\r')fputc(ch,fout);
        fclose(fin);fclose(fout);
        sprintf(R,"rm MEIYONG.txt");
        system(R);
        printf(R);
        putchar('\n');
      
    }
    return 0;
}
```


去'\r'，并把.out改为.ans
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int Zushu=1;
char Name[]="aplusb";
char s1[1001],s2[1001],R[2050];
int main(){
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(int i=1;i<=Zushu;i++){
        sprintf(R,"cp %s%d.in MEIYONG.txt",Name,i);
        system(R);
		printf(R);
		putchar('\n');
		sprintf(R,"%s%d.in",Name,i);
        FILE *fin=fopen("MEIYONG.txt","r"),*fout=fopen(R,"w");
        char ch;
		while((ch=fgetc(fin))!=EOF)if(ch!='\r')fputc(ch,fout);
		fclose(fin);fclose(fout);
		sprintf(R,"rm MEIYONG.txt");
		system(R);
		printf(R);
		putchar('\n');
		
		
		sprintf(R,"cp %s%d.out %s%d.ans",Name,i,Name,i);
        system(R);
		printf(R);
		putchar('\n');
		sprintf(R,"%s%d.out",Name,i);
        fin=fopen(R,"r");
        sprintf(R,"%s%d.ans",Name,i);
	fout=fopen(R,"w");
       // char ch;
		while((ch=fgetc(fin))!=EOF)if(ch!='\r')fputc(ch,fout);
		fclose(fin);fclose(fout);
		sprintf(R,"rm %s%d.out",Name,i);
		system(R);
		printf(R);
		putchar('\n');
        
        
    }
    return 0;
}

```
数据点从0开始编号改为从1开始编号:
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int Zushu=1;
char Name[]="aplusb";
char s1[1001],s2[1001],R[2050];
int main(){
	int judge=system("cls");            // 0 为 Windows，>0 为 Linux
	for(int i=Zushu;i>=1;i--){
		sprintf(R,judge==0?"rename %s%d.in %s%d.in":"mv %s%d.in %s%d.in",Name,i-1,Name,i);
		system(R);
		printf(R);putchar('\n');
		
		sprintf(R,judge==0?"rename %s%d.ans %s%d.ans":"mv %s%d.ans %s%d.ans",Name,i-1,Name,i);
		system(R);
		printf(R);putchar('\n');
	}
	return 0;
}
```

随机分配考号（一行一个名字放在1.csv里，输出考号在player.csv，可以用Excel打开）
```c
#include<stdio.h>
#include<time.h>
#include<stdlib.h>
char sf[]="SD";
char sl[100001];
int v[100001];
int randint(int x,int y){
	return rand()/(y-x+1)+x;
}
int main(){
	srand(time(0)+1000*time(0));
	FILE*fin=fopen("1.csv","r"),*fout=fopen("player.csv","w");
	for(int i=1;fscanf(fin,"%s",sl)!=EOF;i++){
		int s=randint(1,1500);
		while(v[s])s=randint(1,1500);
		v[s]=1;
		printf("%s-%04d,%s\n",sf,s,sl);
		fprintf(fout,"%s-%04d,%s\n",sf,s,sl);
	}
	return 0;
}
```

综合版:
```cpp
#include<bits/stdc++.h>
//#include<dir.h>
#include<unistd.h>
using namespace std;
int Zushu=1;//数据点个数 
char Name[]="aplusb";//题目名称 
char sf[]="SD";//省份(只有需要随机分配考号才用填) 
namespace change_name{
  char s1[1001],s2[1001],R[2050];
  int main(){
    FILE* Fin=fopen("player.csv","r");
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(;;){
        s1[0]=fgetc(Fin);
        if(s1[0]==EOF)break;
        int j;
        for(j=1;s1[j-1]!=',';j++)s1[j]=fgetc(Fin);
        s1[j-1]='\0';
        s2[0]=fgetc(Fin);
        if(s2[0]==EOF)break;
        for(j=1;s2[j-1]!=','&&s2[j-1]!='\r'&&s2[j-1]!='\n';j++)s2[j]=fgetc(Fin);
        int flag=1;
        if(s2[j-1]=='\n')flag=0;
        char Ch=s2[j-1];
        s2[j-1]='\0';

        if(flag){
            Ch=fgetc(Fin);
            while(Ch!='\n'&&Ch!=EOF)Ch=fgetc(Fin);   
        }
        sprintf(R,judge==0?"rename %s%s %s":"mv %s%s %s",s1,s2,s1);
        system(R);
        printf(R);
        putchar('\n');
        if(Ch==EOF)break;
    }
    fclose(Fin);
    //freopen(judge==0?"CON":"/dev/tty","r",stdin);
    return 0;
  }
}
namespace change_name_2{
  char s1[1001],s2[1001],R[2050];
  int main(){
    FILE* Fin=fopen("player.csv","r");
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(;;){
        s1[0]=fgetc(Fin);
        if(s1[0]==EOF)break;
        int j;
        for(j=1;s1[j-1]!=',';j++)s1[j]=fgetc(Fin);
        s1[j-1]='\0';
        s2[0]=fgetc(Fin);
        if(s2[0]==EOF)break;
        for(j=1;s2[j-1]!=','&&s2[j-1]!='\r'&&s2[j-1]!='\n';j++)s2[j]=fgetc(Fin);
        int flag=1;
        if(s2[j-1]=='\n')flag=0;
        char Ch=s2[j-1];
        s2[j-1]='\0';

        if(flag){
            Ch=fgetc(Fin);
            while(Ch!='\n'&&Ch!=EOF)Ch=fgetc(Fin);   
        }
        sprintf(R,judge==0?"rename %s %s":"mv %s %s",s2,s1);
        system(R);
        printf(R);
        putchar('\n');
        if(Ch==EOF)break;
    }
    fclose(Fin);
    //freopen(judge==0?"CON":"/dev/tty","r",stdin);
    return 0;
  }
}
namespace assign_text_number{
  char sl[10001];
  int v[10001];
  int randint(int x,int y){
      return rand()/(y-x+1)+x;
  }
  int main(){
      srand(time(0)+1000*time(0));
      FILE*Fin=fopen("1.csv","r"),*Fout=fopen("player.csv","w");
      for(int i=1;fscanf(Fin,"%s",sl)!=EOF;i++){
          int s=randint(1,1500);
          while(v[s])s=randint(1,1500);
          v[s]=1;
          printf("%s-%04d,%s\n",sf,s,sl);
          fprintf(Fout,"%s-%04d,%s\n",sf,s,sl);
      }
      fclose(Fin);fclose(Fout);
      return 0;
  } 
}
namespace out_to_ans{
  char s1[1001],s2[1001],R[2050];
  int main(){
      int judge=system("cls");            // 0 为 Windows，>0 为 Linux
      for(int i=1;i<=Zushu;i++){
          sprintf(R,judge==0?"rename %s%d.out %s%d.ans":"mv %s%d.out %s%d.ans",Name,i,Name,i);
          system(R);
          printf(R);putchar('\n');
      }
      return 0;
  }
}
namespace del_r{
  char s1[1001],s2[1001],R[2050];
  int main(){
      int judge=system("cls");            // 0 为 Windows，>0 为 Linux
      for(int i=1;i<=Zushu;i++){
          sprintf(R,"cp %s%d.in MEIYONG.txt",Name,i);
          system(R);
          printf(R);
          putchar('\n');
          sprintf(R,"%s%d.in",Name,i);
          FILE *Fin=fopen("MEIYONG.txt","r"),*Fout=fopen(R,"w");
          char ch;
          while((ch=fgetc(Fin))!=EOF)if(ch!='\r')fputc(ch,Fout);
          fclose(Fin);fclose(Fout);
          sprintf(R,"rm MEIYONG.txt");
          system(R);
          printf(R);
          putchar('\n');
  
          sprintf(R,"cp %s%d.ans MEIYONG.txt",Name,i);
          system(R);
          printf(R);
          putchar('\n');
          sprintf(R,"%s%d.ans",Name,i);
          Fin=fopen("MEIYONG.txt","r"),Fout=fopen(R,"w");
         // char ch;
          while((ch=fgetc(Fin))!=EOF)if(ch!='\r')fputc(ch,Fout);
          fclose(Fin);fclose(Fout);
          sprintf(R,"rm MEIYONG.txt");
          system(R);
          printf(R);
          putchar('\n');
  
      }
      return 0;
  }
}
namespace del_r_and_out_to_ans{
  char s1[1001],s2[1001],R[2050];
  int main(){
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(int i=1;i<=Zushu;i++){
        sprintf(R,"cp %s%d.in MEIYONG.txt",Name,i);
        system(R);
        printf(R);
        putchar('\n');
        sprintf(R,"%s%d.in",Name,i);
        FILE *Fin=fopen("MEIYONG.txt","r"),*Fout=fopen(R,"w");
        char ch;
        while((ch=fgetc(Fin))!=EOF)if(ch!='\r')fputc(ch,Fout);
        fclose(Fin);fclose(Fout);
        sprintf(R,"rm MEIYONG.txt");
        system(R);
        printf(R);
        putchar('\n');

        sprintf(R,"cp %s%d.out %s%d.ans",Name,i,Name,i);
        system(R);
        printf(R);
        putchar('\n');
        sprintf(R,"%s%d.out",Name,i);
        Fin=fopen(R,"r");
        sprintf(R,"%s%d.ans",Name,i);
    Fout=fopen(R,"w");
      //  char ch;
        while((ch=fgetc(Fin))!=EOF)if(ch!='\r')fputc(ch,Fout);
        fclose(Fin);fclose(Fout);
        sprintf(R,"rm %s%d.out",Name,i);
        system(R);
        printf(R);
        putchar('\n');

    }
    return 0;
  }
}
namespace _0_to_1{
  char s1[1001],s2[1001],R[2050];
  int main(){
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(int i=Zushu;i>=1;i--){
        sprintf(R,judge==0?"rename %s%d.in %s%d.in":"mv %s%d.in %s%d.in",Name,i-1,Name,i);
        system(R);
        printf(R);putchar('\n');

        sprintf(R,judge==0?"rename %s%d.ans %s%d.ans":"mv %s%d.ans %s%d.ans",Name,i-1,Name,i);
        system(R);
        printf(R);putchar('\n');
    }
    return 0;
  }
}
namespace _0_to_1_out{
  char s1[1001],s2[1001],R[2050];
  int main(){
    int judge=system("cls");            // 0 为 Windows，>0 为 Linux
    for(int i=Zushu;i>=1;i--){
        sprintf(R,judge==0?"rename %s%d.in %s%d.in":"mv %s%d.in %s%d.in",Name,i-1,Name,i);
        system(R);
        printf(R);putchar('\n');

        sprintf(R,judge==0?"rename %s%d.out %s%d.out":"mv %s%d.out %s%d.out",Name,i-1,Name,i);
        system(R);
        printf(R);putchar('\n');
    }
    return 0;
  }
}
namespace File{
  int open(char *NAME,int ZUSHU){
    if(chdir(NAME)==-1){fprintf(stderr,"file worng");exit(0);}
    strcpy(Name,NAME);
    Zushu=ZUSHU;
    return 0;
  }
  int close(){
    if(chdir("..")==-1){fprintf(stderr,"file worng");exit(0);}
    return 0;
  }
}
/*
change_name:更改选手文件名(带考号) 
change_name_2:更改选手文件名(不带考号) 
assign_text_number:随机分配考号(1.csv读取名单)
out_to_ans:.out文件改为.ans文件
del_r:删除'\r'(要在linux运行,改的是.in和.ans)
del_r_and_out_to_ans:删除'\r'并将.out文件改为.ans文件 (要在linux运行)
_0_to_1:将编号从0开始测试点改为编号从1开始(.in .ans)
_0_to_1_out:将编号从0开始测试点改为编号从1开始(.in .out)

File:文件 (.open()打开.close()关闭) 
	(测试点保存在子目录里,若直接放在文件下可直接strcpy给Name赋值=给Zushu赋值)
调用方法:
***::main()
例如: change_name::main() ->调用改文件名函数 
*/
int main(){}

```