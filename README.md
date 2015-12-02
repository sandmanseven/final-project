# final-project
//  Created by Weiqi Wang on 15/12/1.
//  Copyright (c) 2015年 Sandman.Seven. All rights reserved.
#include <iostream>
#include <cmath>
#include <stdio.h>
using namespace std;
typedef char  DATA;
struct CBTType
{
    DATA data;
    CBTType * left;
    CBTType * right;
};
CBTType *InitTree(){
    CBTType *treeNode;
    if (treeNode==new CBTType) {
        cout<<"please enter the first data:"<<endl;
        cin>>treeNode->data;
        treeNode->left=NULL;
        treeNode->right=NULL;
        if (treeNode!=NULL) {
            return treeNode;
        }else {
            return NULL;
        }
    }
    return NULL;
}
CBTType *TreeFindNode(CBTType *treeNode,DATA data){
    CBTType*ptr;
    if(treeNode==NULL){
        return NULL;
    }else
    {
        if (treeNode->data==data) {
            return treeNode;
        }
        else
        {
            if (ptr==TreeFindNode(treeNode->left, data)) {
                return ptr;
            }
            else if(ptr==TreeFindNode(treeNode->right, data)){
                return ptr;
            }
            else
            {
                return NULL;
            }
        }
    }
}
void AddTreeNode(CBTType *treeNode)
{
    CBTType *pnode,*parent;
    DATA data;
    char menusel;
    if(pnode==new CBTType)
    {
        cout<<"please enter the data of the node"<<endl;
        cin>>pnode->data;
        pnode->left=NULL;
        pnode->right=NULL;
        cout<<"please enter the parent data"<<endl;
        cin>>data;
        parent=TreeFindNode(treeNode,data);
        if(!parent)
        {
            cout<<"no parent"<<endl;
            delete pnode;
            return ;
        }
        cout<<"1.add to left；2.add to right and the number "<<endl;
        do
        {
            cin>>menusel;
            if(menusel=='1'||menusel=='2')
            {
                switch(menusel)
                {
                    case '1':
                        if(parent->left)
                        {
                            cout<<"left does not null"<<endl;
                        }
                        else
                        {
                            parent->left=pnode;
                            cout<<"complete "<<endl;
                        }
                        break;
                    case '2':
                        if(parent->right)
                        {
                            cout<<"right does not null"<<endl;
                        } 
                        else
                        {
                            parent->right=pnode;
                            cout<<"coplete！"<<endl;
                        } 
                        break;
                    default:
                        cout<<"pnode is wrong"<<endl;
                        break;
                } 
            }
        }while(menusel!='1'&&menusel!='2');
    }
}
void inOrderTra(CBTType*T){
    if(T!=NULL){
        inOrderTra(T->left);
        printf("%c ",T->data);
        inOrderTra(T->right);
    }
}
