#include <stdio.h>
#include <stdlib.h>
struct student
{
    int num;
    int score;
    struct student *next;
};
int n;
struct student *delet(struct student *head,int num)
{
    struct student *p1,*p2;
    if(head==NULL)
        return head;

    p1=head;
    while(p1->num!=num&&p1->next!=NULL)
    {
        p2=p1;
        p1=p1->next;
    }
    if(num==p1->num)
    {
        if(p1==head)
            head=p1->next;
        else
            p2->next=p1->next;
        printf("delete success!\n");
        n=n-1;
    }
    else
        printf("NOT found");
    return head;
}
struct student *insert(struct student *head,struct student *stud)//照从小到大的顺序排列
{
    struct student *p0,*p1,*p2;
    p1=head;
    p0=stud;
    if(head==NULL)//判断是否是一个空表
    {
        head=p0;//将p0的结点作为头结点
        p0->next=NULL;
    }
    else
    {
        while((p1->next!=NULL)&&(p0->num > p1->num))
        {
            p2=p1;//p2小跟班，指向刚刚p1指向的结点，让p1继续前行
            p1=p1->next;
        }

        if(p0->num <= p1->num)
        {
            if(p1==head)//查到原来第一个节点之前
            {
                head=p0;
                p0->next=p1;
            }
            else//正常情况插入
            {
                p2->next=p0;
                p0->next=p1;
            }
        }
        else//插入到该链表的最后
        {
            p1->next=p0;
            p0->next=NULL;
        }
    }
    n=n+1;//接点数加一
    return head;
};
struct student *creat(void)//返回该链表的头指针
{
    struct student *p1,*p2,*head;//指向结构体类型数据的指针P289
    n=0;//用于判断输入数据个数
    p1=p2=(struct student*)malloc(sizeof(struct student));//分别为两个指针分配新空间
    scanf("%d %d",&p1->num,&p1->score);
    head=NULL;
    while(p1->num!=0)//结束条件
    {
        n=n+1;
        if(n==1)//第一个数据
            head=p1;//头节点接受第一个数据
        else
            p2->next=p1;//将p1数据接在p2数据后面

        p2=p1;//在录入新数据后，p2也指向p1数据
        p1=(struct student*)malloc(sizeof(struct student));//准备接收下一个数据
        scanf("%d %d",&p1->num,&p1->score);

    }
    p2->next=NULL;
    return head;
}
void print(struct student *head)
{
    struct student *p;
    printf("Now,There n records are:%d\n",n);
    p=head;
    if(head!=NULL)
        do
        {
            printf("%d %d\n",p->num,p->score);
            p=p->next;
        }
        while(p!=NULL);
}
int main()
{
    struct student *p,*stu;
    int del_num;
    p=creat();
    print(p);

    /*printf("deleted:");
    scanf("%d",&del_num);
    p=delet(p,del_num);
    print(p);*/
    printf("deleted:");
    scanf("%d",&del_num);
    while(del_num!=0)//设置循环删除停止条件
    {
        p=delet(p,del_num);
        print(p);
        printf("2.deleted:");
        scanf("%d",&del_num);
    }

    printf("input:");
    //在内存的动态存储区分配一个长度为size的连续空间；成功返回分配区域其实地址，不成功返回NULL
    //书上P229 malloc返回的是一个指向任何类型数据的指针（void *类型），所以需要强制转换为struct student类型：(struct student *)
    stu=(struct student *)malloc(sizeof(struct student));
    scanf("%d %d",&stu->num,&stu->score);//无限输出，have question;
    while(stu->num!=0)
    {
        p=insert(p,stu);
        print(p);
        printf("2:input:");
        stu=(struct student *)malloc(sizeof(struct student));
        scanf("%d %d",&stu->num,&stu->score);
    }

}
