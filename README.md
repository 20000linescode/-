# -
一个关于三子棋的游戏
game.h
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>//将stdio.h的头文件加入game.h中只要引用了该头文件则不用再加stdio.h的头文件
#include<stdlib.h>
#include<time.h>
#define ROW 3//宏定义不用加;加了会报错
#define COL 3
//声明两个函数
void InitBoard(char board[ROW][COL],int row,int col);
void Displayboard(char board[ROW][COL],int row,int col);
void PlayerMove(char board[ROW][COL],int row,int col);
void ComputerMove(char board[ROW][COL],int row,int col);
//判断输赢的函数要有以下功能
//玩家赢--'*'
//电脑赢--'#'
//平局---'Q'
//继续游戏--'C'
char IsWin(char board[ROW][COL],int row,int col);
//下面是判断是否平局的函数
int IsFull(char board[ROW][COL],int row,int col);

game.c
#include"game.h"
int IsFull(char board[ROW][COL],int row,int col)//满了返回,没满返回0
{
	int i=0,j=0;
	for(i=0;i<row;i++)
	{
		for(j=0;j<col;j++)
		{
			if(board[i][j]=' ')
				return 0;
		}
	}
	return 1;
}
void InitBoard(char board[ROW][COL],int row,int col)
{
	int i=0,j=0;
	for(i=0;i<row;i++)
	{
		for(j=0;j<col;j++)
		{
			board[i][j]=' ';//全部赋为空格
		}
	}
}
void Displayboard(char board[ROW][COL],int row,int col)
{
	//但是这个代码不够好，因为若要打印不为3*3的棋盘，行会变，但是列不会变
	//int i=0;
	//for(i=0;i<row;i++)//使用for循环打印棋盘
	//{
	//		printf(" %c | %c | %c\n",board[i][0],board[i][1],board[i][2]);
	//		if (i<row-1)//加上if最后一行的---|---|---不打印更为美观
	//		{
	//	       printf("---|---|---\n");

	//		}
	//}
	//下面这个循环可以任意改变ROW,COL.上面的却不行
	int i=0,j=0;
	for(j=0;j<row;j++)//1.最外层循环循环一次，打印一行数据，一行分割行
  {
	  
	  for(i=0;i<col;i++)//2.打印一行的数据
	{
		printf(" %c ",board[j][i]);//注意%c两边要有空格不然不能使棋盘对齐，j=0时，打印三个i,
		if(i<col-1)//少打一个|，美观
		{
			printf("|");
		}
	}
	 printf("\n");//每打印一行数据后就换行，即for循环结束一次换行即可
	 if(j<row-1)//少打印一行分割行，美观
	 {
		for(i=0;i<col;i++)//2.打印一行分割行
	   {
		printf("---");
		if(i<col-1)//少打印结束|，更美观
		{
			printf("|");
		}
	   }
		 printf("\n");//每打印一行分割行后就换行，即for循环结束一次换行即可
	 }
   }
}
void PlayerMove(char board[ROW][COL],int row,int col)
{
	int x=0,y=0;
	printf("玩家走\n");
	while(1)
  {
	printf("请输入坐标:>");
	scanf("%d%d",&x,&y);
	if(x>=1&&x<=row&&y>=1&&y<=col)//由于用户不知道数组下标是从0开始，所以x坐标应该是从1到3
	{
		if(board[x-1][y-1]==' ')//将输入的坐标转化为数组下标
		{                       //若未被占用则赋入*
			board[x-1][y-1]='*';
			break;//所有都合法以后遇到break跳出循环，否则一直会执行while循环，无法停止
		}
		else
		{
			printf("该坐标被占用\n");//否则重新输入，if进来执行到结尾即走完了一个循环
		}
	}
	else
	{
		printf("坐标输入错误请重新输入!\n");//由此可知这是一个循环
	}
  }
   
}
void ComputerMove(char board[ROW][COL],int row,int col)
{
	int x=0,y=0;
	printf("电脑走:>\n");//电脑走的话要生成随机座标即调用rand
	while(1)
	{
	        x=rand()%row;//用<stdlib.h>的头文件引用，再用时间函数生成随机数
	        y=rand()%col;//%上一个row与col将x,y的坐标值控制在了0-2之间，不用再进行判断是否合法
	        if(board[x][y]==' ')//电脑这里不需要进行是否被占用了，如果被占用if不会进入则直接重新产生随机数
	         {
		       board[x][y]='#';//电脑下#号
			   break;//跳出循环
	         }

	}
}
//判断输赢的函数要有以下功能
//玩家赢--'*'
//电脑赢--'#'
//平局---'Q'
//继续游戏--'C'
//35分钟开始。
char IsWin(char board[ROW][COL],int row,int col)
{
	int i=0;
	for(i=0;i<row;i++)//横三行
	{
			if(board[i][0]==board[i][1]&&board[i][1]==board[i][2]&&board[i][1]!=' ')//非常巧妙，i代表行的值，常数代表列的值，i改变可以使三行都改变。
			return board[i][0];
	}
	for(i=0;i<col;i++)//列三行
	{
		if(board[0][i]==board[1][i]&&board[1][i]==board[2][i]&&board[0][i]!=' ')//与列同理
		return board[0][i];
	}
	if(board[0][0]==board[1][1]&&board[1][1]==board[2][2]&&board[0][0]!=' ')//第一条对角线
		return board[0][0];
	if(board[2][0]==board[1][1]&&board[1][1]==board[0][2]&&board[2][0]!=' ')//第二条对角线
		return board[2][0];
	//以上只是判断是否有人赢了，有人赢则会立即返回一个值，很细节
	//以下判断是否平局，平局的条件就是九个格子满了，即平局
	if(1==IsFull(board,ROW,COL))//写一个函数判断是否平局，若平局返回1
	{
			return 'Q';
	}
	else return 'C';
}

test.c
#include"game.h"
void menu()
{
	printf("*****************\n");
	printf("**1.play 0.exit**\n");
	printf("*****************\n");

}
void game()
{
	char ret=0;
	char board[ROW][COL]={0};//一开始初始化为0
	InitBoard(board,ROW,COL);//初始化棋盘，全部放入空格，再用*#，替换
	Displayboard(board,ROW,COL); //打印棋盘
	//下棋
	while(1)
	{
	PlayerMove(board,ROW,COL);//输入坐标后还要打印棋盘即再调用打印棋盘的函数，玩家走
	Displayboard(board,ROW,COL); //打印棋盘
	//判断玩家是否赢的一个函数
	ret=IsWin(board,ROW,COL);
	if(ret!='C')//只要不是C游戏就结束
	{
		break;
	}
	//上面两步是重复执行的所以也要加入循环
	ComputerMove(board,ROW,COL);//电脑走
	Displayboard(board,ROW,COL); //打印棋盘
	//判断电脑是否赢
	ret=IsWin(board,ROW,COL);
	if(ret!='C')
	{
		break;
	}
	}
	//PlayerMove(board,ROW,COL);//输入坐标后还要打印棋盘即再调用打印棋盘的函数
	//Displayboard(board,ROW,COL); //打印棋盘
	//上面两步是重复执行的所以也要加入循环
	if(ret=='*')
	{
		printf("玩家赢\n");
	}
	else if(ret=='#')
	{
		printf("电脑赢\n");
	}
	else
	{
		printf("平局\n");
	}
}
void test()
{
	int input=0;
	//srand((unsigned int)(time(NULL)));//时间戳生成随机数
	do
	{
			menu();
			printf("请选择:>");
			scanf("%d",&input);
			switch(input)
			{
			case 0:
				printf("退出游戏\n");
				break;
			case 1:
				printf("三子棋\n");
				game();
				break;
			default:
				printf("选择错误，请重新选择\n");
				break;
			}

	}while(input);//很细节若为0，则停止，若为1或其他值进行，后期再进行判断
}
int main()
{
	srand((unsigned int)(time(NULL)));//时间戳生成随机数，由于time会产生一个time_t类型的数,所以用一个unsigned int强制转化为int型
	test();
	return 0;
}
