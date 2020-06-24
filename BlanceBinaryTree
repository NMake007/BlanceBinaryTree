#include <iostream>
#include <stack>
#include <cmath>

using namespace std;

typedef int KeyType;
typedef char ElemType;
typedef struct Node
{
    int depth;
    KeyType key;
    ElemType data;
    struct Node *lchild, *rchild;
}Node, *Tree;

class BT
{
public:
    BT() {T = nullptr;}
    Tree search(KeyType k);
    void insert(KeyType k, ElemType e);
    void remove(KeyType k);

protected:
    void Lswap(Tree r, Tree parent);
    void Rswap(Tree r, Tree parent);
    void Countdepth(Tree r);

private:
    Tree T;
};

Tree BT::search(KeyType k)      //搜索到返回节点指针，否则为空指针
{
    auto p = T;
    while (p)
    {
        if (p->key < k)
        {
            p = p->rchild;
        }
        else if (k < p->key)
        {
            p = p->lchild;
        }
        else
        {
            return p;
        }
    }

    return p;
}

void BT::insert(KeyType k, ElemType e)
{
    Tree path[100] = {nullptr};      //保存查找顺序
    int n = -1;          //n为最后一个节点的位置
    int ldep = 0, rdep = 0;     //左右子树的深度
    auto q = new Node;
    q->key = k;
    q->data = e;
    q->lchild = q->rchild = nullptr;
    q->depth = 1;

    if (!T) T = q;
    else        //根节点不为空
    {
        auto p = T;
        while (p)
        {
            if (p->key < k)     //大于当前键值，右孩子循环
            {
                path[++n] = p;
                p = p->rchild;
            }
            else if (k < p->key)    //小于当前键值，左孩子循环
            {
                path[++n] = p;
                p = p->lchild;
            }
            else return;        //存在相同键值，无需插入
        }

        if (path[n]->key < k)   //最后一个节点的键值与插入键值的关系
            path[n]->rchild = q;
        else
            path[n]->lchild = q;

        path[n+1] = q;  //填入用作修改不平衡

        while (n != -1)      //修改各个节点的高度，当遇到不平衡时退出或修改全部节点
        {
            if (path[n]->lchild) ldep = path[n]->lchild->depth;     //左孩子深度
            else ldep = 0;

            if (path[n]->rchild) rdep = path[n]->rchild->depth;     //右孩子深度
            else rdep = 0;

            path[n]->depth = ldep > rdep ? ldep+1 : rdep+1;     //当前节点深度为左右孩子深度的最大值加一
            if (2 == abs(ldep - rdep)) break;
            n--;
        }

        if (n != -1)    //存在不平衡时，n为不平衡树的根
        {
            p = n ? path[n-1] : nullptr;
            if (path[n]->lchild == path[n+1]) //L型
            {
                if (path[n+1]->lchild == path[n+2]) //LL型
                {
                    Lswap(path[n], p);
                }
                else    //LR型
                {
                    Rswap(path[n+1], path[n]);
                    Lswap(path[n], p);
                }
            }
            else            //R型
            {
                if (path[n+1]->rchild == path[n+2]) //RR型
                {
                    Rswap(path[n], p);
                }
                else    //RL型
                {
                    Lswap(path[n+1], path[n]);
                    Rswap(path[n], p);
                }
            }
        }

    }
}

void BT::Lswap(Tree r, Tree parent)
{
    Tree s = r, c = s->lchild;  //根与左孩子互换
    s->lchild = c->rchild;
    c->rchild = s;
    if (parent)     //修改指针
    {
        if (parent->lchild == r) parent->lchild = c;
        else parent->rchild = c;
    }
    else
    {
        T = c;
    }

    Countdepth(s);  //计算深度，顺序不能倒
    Countdepth(c);
}

void BT::Rswap(Tree r, Tree parent)
{
    Tree s = r, c = s->rchild;  //根与右孩子互换
    s->rchild = c->lchild;
    c->lchild = s;
    if (parent)     //修改指针
    {
        if (parent->lchild == r) parent->lchild = c;
        else parent->rchild = c;
    }
    else
    {
        T = c;
    }

    Countdepth(s);  //计算深度，顺序不能倒
    Countdepth(c);
}

void BT::Countdepth(Tree r)
{
    int ldep, rdep;

    if (r->lchild) ldep = r->lchild->depth;     //左孩子深度
    else ldep = 0;

    if (r->rchild) rdep = r->rchild->depth;     //右孩子深度
    else rdep = 0;

    r->depth = ldep > rdep ? ldep+1 : rdep+1;     //当前节点深度为左右孩子深度的最大值加一
}

int main(int argc, char *argv[])
{
    BT b;
    int n = 26;
    for (int i = 1; i <= n; i++)
    {
        b.insert(i, 'a'+i-1);
    }
    for (int i = 1; i <= n; i++)
    {
        cout << b.search(i)->data << endl;
    }
    return 0;
}
