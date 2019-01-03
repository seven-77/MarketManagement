# MarketManagement
#include "stdio.h"         /*输入，输出头文件*/ 
#include "stdlib.h"         /*申请空间头文件*/ 
#include "string.h"         /*对字符串加工头文件*/ 
#include "conio.h"         /*清屏头文件*/ 
FILE *fp;
int n=0;           /*定义文件指针类型*/
int i,j,a[4],m;          /*定义整数类型*/ 
float aver[4],sum[4],g[4],h;      /*定义浮点类型*/ 
 

char c[5]="elec";         /*定义字符数组类型*/
char d[5]="comm";         /*定义字符数组类型*/
char e[5]="food";         /*定义字符数组类型*/
char f[5]="offi";         /*定义字符数组类型*/

struct good           /*定义结构体*/
{
 int num;          /*商品编号*/
 char name[20];         /*商品名称*/
 char kind[40];         /*商品类型*/
 float price;         /*商品价格*/
 char unit[10];         /*商品单位*/
 int quantity;         /*商品数量*/
 struct good *next;        /*定义结构体指针类型*/
}*head,*p1,*p2;

struct good *createlist()                /*创建链表函数*/
{
 struct good *head1,*p1,*p2;         /*定义结构体指针类型*/
 if((fp=fopen("goods message.txt","w"))==NULL)      /*判断能否打开文件*/
 {
  printf("can not open the file");
  exit(0);                  /*结束程序*/
 }
 head1=(struct good *)malloc(sizeof(struct good));     /*申请头结点空间*/ 
 p1=head1;
 p2=head1;
 printf("*********************************************\n");
 printf("请输入信息:编号，名称，类型，价格，单位，数目\n");
 printf("            （以输入“－1”表示结束输入）\n");
 printf("*********************************************\n");
 printf("____________________\n");
 scanf("%d %s %s %f %s %d",&p1->num,p1->name,p1->kind,&p1->price,p1->unit,&p1->quantity);        /*输入商品信息*/ 
 printf("____________________\n");
 p1->next=NULL;
 fprintf(fp,"%d %s %s %f %s %d ",p1->num,p1->name,p1->kind,p1->price,p1->unit,p1->quantity);             /*将商品信息写入文件*/
 while(1)
 {
  p1=(struct good *)malloc(sizeof(struct good));           /*申请新空间*/
  printf("*********************************************\n");
  printf("请输入信息:编号，名称，类型，价格，单位，数目\n");
  printf("            （以输入“－1”表示结束输入）\n");
  printf("*********************************************\n");
  printf("____________________\n");
  scanf("%d",&p1->num);
  if(p1->num==-1)               /*申请空间结束条件*/
  {
   printf("____________________\n\n");
   fprintf(fp,"%d",-1);
   fclose(fp);
   return head1;              /*返回头指针*/
  }
  scanf("%s %s %f %s %d",p1->name,p1->kind,&p1->price,p1->unit,&p1->quantity); /*输入商品信息*/
  printf("________________\n");
  fprintf(fp,"%d %s %s %f %s %d ",p1->num,p1->name,p1->kind,p1->price,p1->unit,p1->quantity);            /*将商品信息写入文件*/
  p1->next=NULL;
  p2->next=p1;
  p2=p1;
 }
}


struct good *paixu(struct good*head2)      /*链表排序函数*/ 
{
 struct good *p6,*p7,*r,*s;       /*定义结构体指针类型*/
 for(i=0;i<=3;i++)            /*赋初值值*/ 
 {
  a[i]=0;
  sum[i]=0;
  aver[i]=0;
 }
 p6=(struct good *)malloc(sizeof(struct good));      /*申请新空间*/
 p6->next=head2;
 head2=p6;
 while(p6->next!=NULL)        /*判断循环结束条件*/
 {
  p7=p6->next;
  r=p6;
  while(p7->next!=NULL)       /*判断循环结束条件*/
  {
   if((p7->next->price)>(r->next->price))    /*判断是否调换*/
   r=p7;
   p7=p7->next;
  }
  if(p6!=r)          /*判断循环结束条件*/
  {
   s=r->next;          /*指针调换*/ 
   r->next=s->next;
   s->next=p6->next;
   p6->next=s;
  }
   p6=p6->next;
 }
 p6=head2;
 head2=head2->next;
 free(p6);           /*释放第一个无效空间*/ 
 return head2;  
}
void jisuan()
{
  p1=head;
 do
 {
  if(strcmp(p1->kind,c)==0)     /*判断是否为电器类型*/            
  {
   sum[0]=sum[0]+(p1->price)*(p1->quantity);  /*求电器总价*/
   a[0]=a[0]+p1->quantity;       /*求电器总件数*/ 
  }
  if(strcmp(p1->kind,d)==0)     /*判断是否为日用品类型*/ 
  {
   sum[1]=sum[1]+(p1->price)*(p1->quantity);  /*求日用品总价*/
   a[1]=a[1]+p1->quantity;       /*求日用品总件数*/ 
  }
  if(strcmp(p1->kind,e)==0)    /*判断是否为办公用品类型*/          
  {
   sum[2]=sum[2]+(p1->price)*(p1->quantity);  /*求办公用品总价*/
   a[2]=a[2]+p1->quantity;      /*求办公用品总件数*/ 
  }
  if(strcmp(p1->kind,f)==0)     /*判断是否为食品类型*/ 
  {
   sum[3]=sum[3]+(p1->price)*(p1->quantity);  /*求食品总价*/
   a[3]=a[3]+p1->quantity;       /*求食品总件数*/ 
  }
  p1=p1->next;
 }while (p1!=NULL);        /*遍历链表结束条件*/
 for(i=0;i<4;i++)
  aver[i]=sum[i]/a[i];       /*求每类商品平均价*/
 printf("****************************************************\n");
 printf("商品类型     \t    平均价\t       总库存量\n");
 printf("****************************************************\n");
 printf("____________________________________________________\n");
 printf("电器总价值:%0.1f\t平均价:%0.1f\t总库存量:%d\n",sum[0],aver[0],a[0]);
 printf("____________________________________________________\n");
 printf("日用品总价值:%0.1f\t平均价:%0.1f\t总库存量:%d\n",sum[1],aver[1],a[1]);
 printf("____________________________________________________\n");
 printf("食品总价值:%0.1f\t平均价:%0.1f\t总库存量:%d\n",sum[2],aver[2],a[2]);
 printf("____________________________________________________\n");
 printf("办公用品总价值:%0.1f\t平均价:%0.1f\t总库存量:%d\n",sum[3],aver[3],a[3]);
 printf("____________________________________________________\n");
}


void shuchu()           /*输出商品信息函数*/
{ 
 do
 {
  struct good *p3,*p4,*p5;      /*定义结构体指针类型*/
  int n=0,p=0,q=0,r=0;
  printf("所有商品信息：\n");
  printf("编号，名称，类型，价格，单位，数目\n");
  printf("**********************************\n"); 
  if((fp=fopen("goods message.txt","rb+"))==NULL) /*判断能否打开文件*/
  {
   printf("can not open the file");
   exit(0);          /*结束程序*/
  }
  head=(struct good *)malloc(sizeof(struct good)); /*申请头结点空间*/ 
  p3=head;
  fscanf(fp,"%d %s %s %f %s %d ",&p3->num,p3->name,p3->kind,&p3->price,p3->unit,&p3->quantity);             /*从文件中写到链表*/   
  while(1)
  {
   p4=(struct good *)malloc(sizeof(struct good));  /*申请头结点空间*/ 
   fscanf(fp,"%d ",&p4->num);
   if(p4->num!=-1)        /*判断循环结束条件*/
   {
    fscanf(fp,"%s %s %f %s %d ",p4->name,p4->kind,&p4->price,p4->unit,&p4->quantity); /*从文件中写到链表*/ 
    p4->next=NULL;
    p3->next=p4;
    p3=p4;
   }
   else
   {
    p3->next=NULL;
    break;
   }
  }
  fclose(fp);           /*关闭文件*/         
  p3=head;
  while(p3!=NULL)
  { 
   printf("   %d   %s   %s   %0.1f   %s   %d\n\n",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity); 
   printf("__________________________________\n");  
   p3=p3->next;
  }
   printf("**********************************\n"); 
   printf("//////////////////////////////////\n"); 
      while(n!=4)
  {
   p3=head;
   printf("**********************************\n"); 
   printf("1 添加商品信息\n");
   printf("2 删除某商品信息\n");
   printf("3 修改某商品信息\n");
   printf("4 返回（当你完成了对某一商品的添加、删除或者修改后请按4返回）\n");
   printf("**********************************\n");    
   scanf("%d",&n);
   if(n==1)             /*添加商品信息*/
   { 
    printf("请输入商品 编号  名称  类型  价格  单位  数目\n");
    printf("**********************************\n"); 
    p4=(struct good *)malloc(sizeof(struct good));     /*申请空间*/
    scanf("%d %s %s %f %s %d",&p4->num,p4->name,p4->kind,&p4->price,p4->unit,&p4->quantity);         /*输入商品信息*/ 
    p4->next=NULL;
    while(p3->next!=NULL)        /*判断循环结束条件*/
    {
     p3=p3->next;
    }
    p3->next=p4;
    p3=head;
    if((fp=fopen("goods message.txt","w"))==NULL)               /*判断能否打开文件*/
    {
     printf("can not open the file");
     exit(0);             /*结束程序*/
    }
    while(p3!=NULL)
    {
     fprintf(fp,"%d %s %s %f %s %d ",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity)                  /*将商品信息写入文件*/
     p3=p3->next;
    }
    fprintf(fp,"%d",-1);
    fclose(fp);          /*关闭文件*/
    printf("**********************************\n"); 
    printf("__________________________________\n");
    printf("------------请按4返回-------------\n");
    printf("__________________________________\n");
    printf("**********************************\n");     
   }
   if(n==2)            /*删除商品*/
   {
    printf("**********************************\n");  
    printf("请输入需要删除的商品编号\n");
    printf("**********************************\n"); 
    scanf("%d",&p);
    printf("**********\n"); 
    printf("1 确认删除\n2 取消删除\n");
    printf("**********\n"); 
    scanf("%d",&r);
    if(r==1)
    {
     if((head->num)==p)
     {
      head=head->next;
      free(p3);         /*释放空间*/
     }
     else
     {
      p4=head;
      p3=p4->next;
      while(p3!=NULL)          /*判断循环结束条件*/
      {
       if((p3->num)==p)
       {
        p5=p3->next;
        free(p3);       /*释放空间*/
        p4->next=p5;
        break;
       }
       p3=p3->next;
       p4=p4->next;
      }
     }
     if((fp=fopen("goods message.txt","w"))==NULL)         /*判断能否打开文件*/
     {
      printf("can not open the file");
      exit(0);         /*结束程序*/
     }
     p3=head;
     while(p3!=NULL)          /*判断循环结束条件*/
     {
      fprintf(fp,"%d %s %s %f %s %d ",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity);             /*将商品信息写入文件*/
      p3=p3->next;
     }
     fprintf(fp,"%d",-1);
     fclose(fp);         /*关闭文件*/
    }
    if(r==2)
     continue;         /*继续循环*/
    printf("**********************************\n"); 
    printf("__________________________________\n");
    printf("------------请按4返回-------------\n");
    printf("__________________________________\n");
    printf("**********************************\n");    

   }

   if(n==3)           /*修改某商品信息*/
   {
    printf("请输入需要修改的商品编号\n");
    scanf("%d",&q);
    while(p3!=NULL)      /*判断循环结束条件*/
    {
     if((p3->num)==q)       /*判断是否为所需要修改的商品*/ 
     {
      printf("请输入商品单价与库存量（如果单价不变请输入原来的单价）\n");
      scanf("%f %d",&p3->price,&p3->quantity);             /*输入商品价格与库存量*/ 
     }
     p3=p3->next;
    }
    if((fp=fopen("goods message.txt","w"))==NULL)               /*判断能否打开文件*/
    {
     printf("can not open the file");
     exit(0);             /*结束程序*/
    }
    p3=head;
    while(p3!=NULL)     /*判断循环结束条件*/
    {
     fprintf(fp,"%d %s %s %f %s %d ",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity);               /*将商品信息写入文件*/
     p3=p3->next;
    }
    fprintf(fp,"%d",-1);
    fclose(fp);            /*关闭文件*/
    printf("**********************************\n"); 
    printf("__________________________________\n");
    printf("------------请按4返回-------------\n");
    printf("__________________________________\n");
    printf("**********************************\n");    
   }
   if(n==4)                 /*退出*/
    break;
   }
  printf("**********\n");  
  printf("1 继续修改\n---------\n2 返回\n");
  printf("**********\n"); 
  scanf("%d",&p);
  if(p==1)
   continue;        /*继续循环*/
  if(p==2)
   break;         /*跳出循环*/
 }while(n!=2);
 fclose(fp);          /*关闭文件*/
}

void printf0(struct good *p)     /*遍历链表并打印电器类商品函数*/
{
 struct good *p3;        /*定义结构体指针类型*/
 p3=p;
 while (p3!=NULL)       /*判断遍历链表循环结束条件*/
 {
  if(strcmp(p3->kind,c)==0)   /*判断商品类型是否为电器类型*/
  {
   printf("%d\t%s\t%s\t%0.1f\t%s\t%d\n",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity);                    /*输出电器类商品信息*/
   printf("________________________________________________\n");
  }
  p3=p3->next;
 }
 return;
}

void printf1(struct good *p)       /*遍历链表并打印日用品类商品函数*/
{
 struct good *p3;           /*定义结构体指针类型*/
 p3=p; 
 while (p3!=NULL)       /*判断遍历链表循环结束条件*/
 {
  if(strcmp(p3->kind,d)==0)   /*判断商品类型是否为日用品类型*/
  {
   printf("%d\t%s\t%s\t%0.1f\t%s\t%d\n",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity);                     /*输出日用品类商品信息*/
   printf("________________________________________________\n");
  }
  p3=p3->next;
 }
 return;
}

void printf2(struct good *p)    /*遍历链表并打印办公用品类商品函数*/
{
 struct good *p3;        /*定义结构体指针类型*/
 p3=p;
 while (p3!=NULL)       /*判断遍历链表循环结束条件*/
 {
  if(strcmp(p3->kind,e)==0)  /*判断商品类型是否为办公用品类型*/
  {
   printf("%d\t%s\t%s\t%0.1f\t%s\t%d\n",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity);                    /*输出办公用品类商品信息*/
   printf("________________________________________________\n");
  }
  p3=p3->next;
 }
 return;
}

void printf3(struct good *p)        /*遍历链表并打印食品类商品函数*/
{
 struct good *p3;          /*定义结构体指针类型*/
 p3=p;
 while (p3!=NULL)      /*判断遍历链表循环结束条件*/
 { 
  if(strcmp(p3->kind,f)==0)   /*判断商品类型是否为食品类型*/
  {
   printf("%d\t%s\t%s\t%0.1f\t%s\t%d\n",p3->num,p3->name,p3->kind,p3->price,p3->unit,p3->quantity);                   /*输出食品类商品信息*/
   printf("________________________________________________\n");
  }
  p3=p3->next;
 }
 return;
}

void shunxudayin()
{
  for(i=0;i<4;i++)
  g[i]=aver[i];        /*将平均价赋给新数组*/
 for(j=0;j<3;j++)        /*将新数组用冒泡排序法排序*/
  for(i=j+1;i<4;i++)
  {
   if(g[j]<g[i])
   {
    h=g[j];
    g[j]=g[i];
    g[i]=h;
   }
  }
 printf("\n****************************\n");
 printf("商品平均价格排序表（从高到低）\n");
 printf("****************************\n");
 printf("________________________________________________\n");
 printf("编号\t名称\t类别\t单价\t单位\t数量\n");
 printf("________________________________________________\n");
 for(j=0;j<4;j++)
  for(i=0;i<4;i++)
  {
   if (aver[i]==g[j])    /*判断每类商品平均价格的先后顺序*/
    switch(i)
    {
    case 0:
     printf0(head); /*调用遍历链表并打印电器类商品函数*/
     break;
    case 1:
     printf1(head); /*调用遍历链表并打印日用品类商品函数*/
     break;
    case 2:
     printf2(head);/*调用遍历链表并打印办公用品类商品函数*/
     break;
    case 3:
     printf3(head);  /*调用遍历链表并打印食品类商品函数*/
     break;
    }
  }
}

void tongji1()
{
 p1=head;
 printf("\n************************\n");
 printf("库存量低于100的货名及类别\n");
 printf("************************\n");
 printf("________________________\n");
 printf("商品名称\t商品类型\n");
 printf("________________________\n");
 while(p1!=NULL)       /*判断遍历链表循环结束条件*/
 {
  if(p1->quantity<100)      /*判断库存量是否小于100*/
  {
   printf("%s\t%s\n",p1->name,p1->kind); /*输出商品名称及类别*/
   printf("________________________\n");
  }
  p1=p1->next;
 }
}

void tongji2()
{
  printf("\n**********************************************\n");
 printf("商品库存量有2种以上（含2种）低于100的商品类别:\n");
 printf("**********************************************\n");
 printf("________\n");
 if((a[0]<100)&&(a[0]>=2))                              /*判断电器类库存量是否为2种以上（含2种）低于100*/
 {
  printf("电器\n");
  printf("________\n");
 }
 if((a[1]<100)&&(a[1]>=2))                              /*判断日用品类库存量是否为2种以上（含2种）低于100*/
 {
  printf("日用品\n");
  printf("________\n");
 }
 if((a[2]<100)&&(a[2]>=2))                               /*判断食品类库存量是否为2种以上（含2种）低于100*/
 {
  printf("食品\n");
  printf("________\n");
 }
 if((a[3]<100)&&(a[3]>=2))                              /*判断办公用品类库存量是否为2种以上（含2种）低于100*/
 {
  printf("办公用品\n");
  printf("________\n");
 }
}

int main(int argc, char* argv[])
{
 struct good *p1,*p2;             /*定义结构体指针类型*/ 
 while(1)
 {
  printf("***********************************************\n");
  printf("1 ----------输出查看或者修改已存信息-----------\n");
  printf("-----------------------------------------------\n");
  printf("2 -----重新输入新信息（并且删除原有信息）------\n");
  printf("-----------------------------------------------\n");
  printf("3 统计商品信息（如果您还没有查看过信息请先按1）\n"); 
  printf("-----------------------------------------------\n");
  printf("4 -------------------退出---------------------\n");
  printf("***********************************************\n");
  scanf("%d",&m);
  if(m==1)
   shuchu();         /*调用输出信息函数*/
  if(m==2)
  {
   system("cls");
   head=createlist();       /*调用建立链表函数*/ 
  }
  if(m==3)
  {
   printf("统计结果如下\n");  
   head=paixu(head);       /*调用链表排序函数*/        
   jisuan();          /*调用计算函数*/ 
   shunxudayin();        /*调用顺序打印函数*/ 
   tongji1();          /*调用统计1函数*/ 
   tongji2();          /*调用统计2函数*/ 
  }
  if(m==4)
  {
   p1=head;
   while(p1!=NULL)      /*判断遍历链表结束条件*/
   {
    p2=p1->next;
    free(p1);         /*释放空间*/
    p1=p2;
   }
   break;
  }
 }
 return 0;          /*结束程序*/
} 
