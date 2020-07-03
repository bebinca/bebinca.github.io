---
layout: post
title:  TinyXML2 获取信息方面的使用
date:   2020-07-03 08:01:00 +0800
categories: TinyXML2
---

* content
{:toc}
# TinyXML2 获取信息方面的使用

主要是为了oop大程学一下方便记录。

https://www.cnblogs.com/happykoukou/p/6307257.html

用户数据表设计如下：

| 变量名   | 描述     | 类型    | 长度（字节） | 不为空 | 主键 |
| -------- | -------- | ------- | ------------ | ------ | ---- |
| UserName | 用户名   | Vchar   | 3-20         | Y      | Y    |
| Password | 密码     | Char    | 32           | Y      | N    |
| Gender   | 性别     | Int     | 1            | N      | N    |
| Mobile   | 电话     | Char    | 11           | N      | N    |
| Email    | 电子邮箱 | Varchar | 1-50         | N      | N    |

对应XML文件实现：

## 查询xml文件的指定节点

Xml文件中，一个用户节点存储一个用户的信息。因此，对用户信息的增删查改，即无论查询节点、删除节点、修改节点和增加节点，都需要获取需要操作的节点。那么先实现一个根据用户名获取节点指针的函数：

```c++
//function:根据用户名获取用户节点
//param:root:xml文件根节点；userName：用户名
//return：用户节点
XMLElement* queryUserNodeByName(XMLElement* root,const string& userName)
{

    XMLElement* userNode=root->FirstChildElement("User");
    while(userNode!=NULL)
    {
        if(userNode->Attribute("Name")==userName)
            break;
        userNode=userNode->NextSiblingElement();//下一个兄弟节点
    }
    return userNode;
}
```

在以上函数的基础上，获取用户信息的函数：

```c++
User* queryUserByName(const char* xmlPath,const string& userName)
{
    XMLDocument doc;
    if(doc.LoadFile(xmlPath)!=0)
    {
        cout<<"load xml file failed"<<endl;
        return NULL;
    }
    XMLElement* root=doc.RootElement();
    XMLElement* userNode=queryUserNodeByName(root,userName);

    if(userNode!=NULL)  //searched successfully
    {
        User* user=new User();
        user->userName=userName;
        user->password=userNode->Attribute("Password");
        XMLElement* genderNode=userNode->FirstChildElement("Gender");
        user->gender=atoi(genderNode->GetText());
        XMLElement* mobileNode=userNode->FirstChildElement("Mobile");
        user->mobile=mobileNode->GetText();     
        XMLElement* emailNode=userNode->FirstChildElement("Email");
        user->email=emailNode->GetText();           
        return user;
    }
    return NULL;
}
```

## 获取xml文件申明

```c++
//function:获取xml文件申明
//param:xmlPath:xml文件路径；strDecl：xml申明
//return：bool
bool getXMLDeclaration(const char* xmlPath,string& strDecl)
{
    XMLDocument doc;
    if(doc.LoadFile(xmlPath)!=0)
    {
        cout<<"load xml file failed"<<endl;
        return false;
    }
    XMLNode* decl=doc.FirstChild();  
    if (NULL!=decl)  
    {  
        XMLDeclaration* declaration =decl->ToDeclaration();  
        if (NULL!=declaration)  
        {  
              strDecl = declaration->Value();
              return true;
        } 
    }
    return false;
}
```

验证：

```c++
int main(int argc,char* argv[])
{
    //获取xml文件申明
    string strDecl;
    if(getXMLDeclaration("./user.xml",strDecl))
    {
        cout<<"declaration:"<<strDecl<<endl;
    }
    return 0;
}
```

验证结果：

```c++
declaration:xml version="1.0" encoding="UTF-8" standalone="no"
```

## 打印xml文件至标准输出

```c++
//function:将xml文件内容输出到标准输出
//param:xmlPath:xml文件路径
void print(const char* xmlPath)
{
    XMLDocument doc;
    if(doc.LoadFile("./user.xml")!=0)
    {
        cout<<"load xml file failed"<<endl;
        return;
    }
    doc.Print();
}
```

## 解析xml格式的字符串信息

```c++
const char * xmlString = "<?xml version=\"1.0\" standalone=no>\n<!– Our to do list data –>\n<ToDo>\n<Item priority=\"1\"> <bold>Toy store!</bold>\n</Item>\n<Item priority=\"2\"> Do bills</Item>\n</ToDo> ";
XMLDocument *doc = new XMLDocument();
doc->Parse(xmlString);
 
XMLElement * rootElement = doc->RootElement();
const char * rootName = rootElement->Value();
```

