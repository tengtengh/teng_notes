

华为2022年4月6日晚19：00机试第一题：
题目：
### 1、查找舆情热词
网上的新闻越来越多，希望对新闻进行热词处理并归类，方便获取信息。现在已经将每篇新闻处理为2个字符串，即一个标题串一个正文串，字符串中使用" "作为分个词语的分隔符进行分词。
M篇新闻按照新闻法步的先后顺序处理完并输入，现在希望对所有新闻中出现的词语进行处理，输出出现频率最高的topN个词语，作为热词。
标题中出现的词语频率系数为3，正文中出现的词语频率系数为1；返回答案中出现的频率由高到低排序，
当词语出现的频率相同时，在标题中出现的频率次数高的排在前面；
如果过仍然相同，则按照词语在标题中出现的先后顺序进行排序，先出现的排在前面；
如果仍然相同，则按照词语在正文中国出现的先后顺序进行排序，先出现的排在前面

**解答要求**

时间限制：c/c++ 2000ms，其它语言4000ms
内存限制：c/c++ 256MB，其它语言512MB

**输入**

第一行输入为正整数topN和文章数M，即要输出的出现频率最高的词语的个数和处理的文章的数量，由于每篇文章被处理为标题和正文两行，因此后面有2*
行数据。
从第二行起是按顺序处理后每篇文章的标题串和正文串，即第二行是第一篇文章的标题串，第三行是第一篇文章的正文串，第四行是第二篇文章的标题串，地入行是第二篇文章的正文串，以此类推。
参数限制如下：
0 < topN < 1000
0 < M < 100000
0 < 每篇文章的词语数 < 5000

**输出**

使用一行输出出现频率最高的topN个词语，每个词语以" "隔开。

**样例1**

```
input: 
3 2
xinguan feiyan xinzeng bendi quezhen anli
ju baodao chengdu xinzeng xinguan feiyan bendi quezhen anli yili shenzhen xinzeng bendi quezhen anli liangli yiqing zhengti kongzhi lianghao
xinguan yimiao linchuang shiyan
wuzhong xinguan yimiao tongguo sanqi linchuang shiyan xiaoguo lianghao

output:
xinguan xinzeng bendi

Example:
各词语出现的频率：
xinguan = 2 * 3 + 2 = 8
xinzeng = 1 * 3 + 2 = 5
bendi = 1 * 3 + 2 = 5
anli = 1 * 3 + 2 = 5
quezhen = 1 * 3 + 2 = 5
yimiao = 1 * 3 + 1 = 4
feiyan = 1 * 3 + 1 = 4
linchuang = 1 * 3 + 1 = 4
shiyan = 1 * 3 + 1 = 4
返回出现频率最高的3个词语，有4个词语出现的频率都为5，标题出现频率都为3，所以选择先出现的两个词语"xinzeng"和"bendi" 
```


**我写的代码：**(未精简版本，但是比较好理解，可以简化的地方其实很多)

```c++
#include<unordered_map>
#include <iostream>
#include <vector>
// #include<algorithm>
#include<string.h>
#include<string>
#include<math.h>
// #include<stdlib.h>
using namespace std;

void coutTest(unordered_map<string, vector<vector<int>>> umap){
    for(auto it = umap.begin(); it != umap.end(); it++){
        cout << it->first << " | ";
        for(int i : it->second[0]){
            cout << i << " ";
        }
        cout << " | ";
        for(int i : it->second[1]){
            cout << i << " ";
        }
        cout << endl;
    }
}

// void bubble_sort(int A[], int n){
//     for(bool sorted = false; sorted = !sorted; n--)
//         for(int i = 1; i < n; i++){
//             if(A[i - 1] > A[i]){
//                 swap(A[i - 1], A[i]);
//                 sorted = false;
//             }
//     }
// }

// 对allstrs进行起泡法排序，
// 排序的规则是依据umap的vector 的比较
void mysorted(vector<string> &allstrs, unordered_map<string, vector<vector<int>>>& umap){
    int n = allstrs.size();
    for(bool sorted = false; sorted = !sorted; n--){
        for(int i = 1; i < n; i++){

            ///////////排序规则,vector交换的依据////////////
            if(umap[allstrs[i - 1]][0].size() * 3 + umap[allstrs[i - 1]][1].size() < umap[allstrs[i]][0].size() * 3 + umap[allstrs[i]][1].size()){
                swap(allstrs[i - 1],allstrs[i]);
                sorted = false;
            }else if(umap[allstrs[i - 1]][0].size() * 3 + umap[allstrs[i - 1]][1].size() == umap[allstrs[i]][0].size() * 3 + umap[allstrs[i]][1].size()){
                if(umap[allstrs[i - 1]][0].size() < umap[allstrs[i]][0].size()){
                    swap(allstrs[i - 1],allstrs[i]);
                    sorted = false;
                }else if(umap[allstrs[i - 1]][0].size() == umap[allstrs[i]][0].size()){
                    
                    if(umap[allstrs[i - 1]][0].size() != 0 && umap[allstrs[i]][0][0] < umap[allstrs[i]][0][0]){
                        swap(allstrs[i - 1],allstrs[i]);
                        sorted = false;
                    }else if(umap[allstrs[i - 1]][0].size() == 0){
                        if(umap[allstrs[i - 1]][1].size() != 0 && umap[allstrs[i]][1][0] < umap[allstrs[i]][1][0]){
                            swap(allstrs[i - 1],allstrs[i]);
                            sorted = false;
                        }
                    }
                }
            }
            //////////////////////////////////////////////
        }
    }
}


//split;
string killQuestion1(string title, string text, int n){
    unordered_map<string, vector<vector<int>>> umap;
    //vector<vector<int>> [0]:在title中出现的位次 [1]正文中
    //例如，在合并后的标题中词语"xinguan"出现在第0、6个单词的位置
    ////   在合并后的正文中词语"xinguan"出现在第4、21个单词的位置
    // 那么在unordered_map中对应的就是：
    // it->first : "xinguan"
    // it->second : {{0, 6}, {4, 21}}
    string curstr;
    int i_title = 0;//标题中出现的单词位置
    for(int i = 0; i < title.size() + 1; i++){
        if(i == title.size() || title[i] == ' '){
            umap[curstr].resize(2);
            umap[curstr][0].push_back(i_title);
            curstr.clear();
            i_title++;
        }else{
            curstr.push_back(title[i]);
        }
    }
    int i_text = 0;//正文中出现的单词位置
    curstr.clear();
    for(int i = 0; i < text.size() + 1; i++){
        if(i == text.size() || text[i] == ' '){
            umap[curstr].resize(2);
            umap[curstr][1].push_back(i_text);
            curstr.clear();
            i_text++;
        }else{
            curstr.push_back(text[i]);
        }
    }

    ////////test1
    cout << endl;
    cout << "-----test1 start----" << endl;
    cout << "-----cout umap----" << endl;
    coutTest(umap);
    cout << "-----test1 endl----" << endl;
    /////////


    vector<string> allstrs;
    for(auto it = umap.begin(); it != umap.end(); it++){
        allstrs.push_back(it->first);
    }
    
    /////////test2
    cout << endl;
    cout << "-----test2 start----" << endl;
    cout << "---测试输出排序前的allstrs---" << endl;
    for(auto str_tmp : allstrs){
        cout << str_tmp << " ";
    }
    cout << endl<< endl;
    cout << "---test2 end----" << endl;
    /////////

    mysorted(allstrs, umap);

    /////////test3
    cout << endl;
    cout << "---test3 start----" << endl;
    cout << "---测试输出排序后的allstrs---" << endl;
    for(auto str_tmp : allstrs){
        cout << str_tmp << " ";
    }
    cout << endl << endl;
    cout << "---test3 end----" << endl;
    /////////



    ///////test4
    cout << endl;
    cout << "---test4 start----" << endl;
    cout << "---按顺序输出各个词出现的频率----" << endl;
    for(auto str_tmp: allstrs){
        cout << str_tmp << " = ";
        cout << umap[str_tmp][0].size() << " * 3 + ";
        cout << umap[str_tmp][1].size() << " = ";
        cout << umap[str_tmp][0].size() * 3 + umap[str_tmp][1].size() << endl;

    }
    cout << "---test4 end----" << endl;

    ///////////


    //答案输出前n个结果
    string res;
    for(int i = 0; i < n; i++){
        res += allstrs[i] + " ";
    }
    res.pop_back();
    


    return res;
}



void test_demo(){
   int topicN, M;
    cin >> topicN;
    cin.get();
    cin >> M;
    cin.get();


    //读入所有的title和text到title_s和text_s
    string title_s[M];
    string text_s[M];
    int i = 0;
    while(i < M){
        getline(cin, title_s[i]);
        getline(cin, text_s[i]);
        i++;
    }

    // 将title_s和text_s中的所有字符串分别合成一个title、text字符串
    ////其实这个地方就是可以改成直接读进一个字符串，
    //////////或者将title_s和text_s作为函数killQuestion1的输入，但是我没改
    i = 0;
    string title;
    string text;
    while(i < M){
        title += title_s[i] + ' ';
        text += text_s[i] + ' ';
        
        i++;
    }
    title.pop_back();
    text.pop_back();
    cout << "topicN = " << topicN << " M = " << M << endl;;
    cout << "title : " << title << endl;
    cout << "text : " << text << endl;

    cout << killQuestion1(title, text, topicN) << endl;

}


int main(void){
    cout << "=============================" << endl;
    cout << " solution ->->-> start ->->->" << endl;
    test_demo();

    cout << " solution ->->-> end ->->->" << endl;
    cout << "=============================" << endl;
    return 0;
}
```


terminal

```shell
=============================
 solution ->->-> start ->->->
3 2
xinguan feiyan xinzeng bendi quezhen anli
ju baodao chengdu xinzeng xinguan feiyan bendi quezhen anli yili shenzhen xinzeng bendi quezhen anli liangli yiqing zhengti kongzhi lianghao
xinguan yimiao linchuang shiyan
wuzhong xinguan yimiao tongguo sanqi linchuang shiyan xiaoguo lianghao
topicN = 3 M = 2
title : xinguan feiyan xinzeng bendi quezhen anli xinguan yimiao linchuang shiyan
text : ju baodao chengdu xinzeng xinguan feiyan bendi quezhen anli yili shenzhen xinzeng bendi quezhen anli liangli yiqing zhengti kongzhi lianghao wuzhong xinguan yimiao tongguo sanqi linchuang shiyan xiaoguo lianghao

-----test1 start----
-----cout umap----
xiaoguo |  | 27 
sanqi |  | 24 
wuzhong |  | 20 
zhengti |  | 17 
yiqing |  | 16 
liangli |  | 15 
lianghao |  | 19 28 
yimiao | 7  | 22 
tongguo |  | 23 
yili |  | 9 
xinzeng | 2  | 3 11 
ju |  | 0 
shenzhen |  | 10 
bendi | 3  | 6 12 
feiyan | 1  | 5 
baodao |  | 1 
anli | 5  | 8 14 
chengdu |  | 2 
linchuang | 8  | 25 
kongzhi |  | 18 
quezhen | 4  | 7 13 
xinguan | 0 6  | 4 21 
shiyan | 9  | 26 
-----test1 endl----

-----test2 start----
---测试输出排序前的allstrs---
xiaoguo sanqi wuzhong zhengti yiqing liangli lianghao yimiao tongguo yili xinzeng ju shenzhen bendi feiyan baodao anli chengdu linchuang kongzhi quezhen xinguan shiyan 

---test2 end----

---test3 start----
---测试输出排序后的allstrs---
xinguan xinzeng bendi anli quezhen yimiao feiyan linchuang shiyan lianghao xiaoguo sanqi wuzhong zhengti yiqing liangli tongguo yili ju shenzhen baodao chengdu kongzhi 

---test3 end----

---test4 start----
---按顺序输出各个词出现的频率----
xinguan = 2 * 3 + 2 = 8
xinzeng = 1 * 3 + 2 = 5
bendi = 1 * 3 + 2 = 5
anli = 1 * 3 + 2 = 5
quezhen = 1 * 3 + 2 = 5
yimiao = 1 * 3 + 1 = 4
feiyan = 1 * 3 + 1 = 4
linchuang = 1 * 3 + 1 = 4
shiyan = 1 * 3 + 1 = 4
lianghao = 0 * 3 + 2 = 2
xiaoguo = 0 * 3 + 1 = 1
sanqi = 0 * 3 + 1 = 1
wuzhong = 0 * 3 + 1 = 1
zhengti = 0 * 3 + 1 = 1
yiqing = 0 * 3 + 1 = 1
liangli = 0 * 3 + 1 = 1
tongguo = 0 * 3 + 1 = 1
yili = 0 * 3 + 1 = 1
ju = 0 * 3 + 1 = 1
shenzhen = 0 * 3 + 1 = 1
baodao = 0 * 3 + 1 = 1
chengdu = 0 * 3 + 1 = 1
kongzhi = 0 * 3 + 1 = 1
---test4 end----
xinguan xinzeng bendi
 solution ->->-> end ->->->
=============================
```