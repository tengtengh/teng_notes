

��Ϊ2022��4��6����19��00���Ե�һ�⣺
��Ŀ��
### 1�����������ȴ�
���ϵ�����Խ��Խ�࣬ϣ�������Ž����ȴʴ������࣬�����ȡ��Ϣ�������Ѿ���ÿƪ���Ŵ���Ϊ2���ַ�������һ�����⴮һ�����Ĵ����ַ�����ʹ��" "��Ϊ�ָ�����ķָ������зִʡ�
Mƪ���Ű������ŷ������Ⱥ�˳�����겢���룬����ϣ�������������г��ֵĴ�����д����������Ƶ����ߵ�topN�������Ϊ�ȴʡ�
�����г��ֵĴ���Ƶ��ϵ��Ϊ3�������г��ֵĴ���Ƶ��ϵ��Ϊ1�����ش��г��ֵ�Ƶ���ɸߵ�������
��������ֵ�Ƶ����ͬʱ���ڱ����г��ֵ�Ƶ�ʴ����ߵ�����ǰ�棻
�������Ȼ��ͬ�����մ����ڱ����г��ֵ��Ⱥ�˳����������ȳ��ֵ�����ǰ�棻
�����Ȼ��ͬ�����մ����������й����ֵ��Ⱥ�˳����������ȳ��ֵ�����ǰ��

**���Ҫ��**

ʱ�����ƣ�c/c++ 2000ms����������4000ms
�ڴ����ƣ�c/c++ 256MB����������512MB

**����**

��һ������Ϊ������topN��������M����Ҫ����ĳ���Ƶ����ߵĴ���ĸ����ʹ�������µ�����������ÿƪ���±�����Ϊ������������У���˺�����2*
�����ݡ�
�ӵڶ������ǰ�˳�����ÿƪ���µı��⴮�����Ĵ������ڶ����ǵ�һƪ���µı��⴮���������ǵ�һƪ���µ����Ĵ����������ǵڶ�ƪ���µı��⴮���������ǵڶ�ƪ���µ����Ĵ����Դ����ơ�
�����������£�
0 < topN < 1000
0 < M < 100000
0 < ÿƪ���µĴ����� < 5000

**���**

ʹ��һ���������Ƶ����ߵ�topN�����ÿ��������" "������

**����1**

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
��������ֵ�Ƶ�ʣ�
xinguan = 2 * 3 + 2 = 8
xinzeng = 1 * 3 + 2 = 5
bendi = 1 * 3 + 2 = 5
anli = 1 * 3 + 2 = 5
quezhen = 1 * 3 + 2 = 5
yimiao = 1 * 3 + 1 = 4
feiyan = 1 * 3 + 1 = 4
linchuang = 1 * 3 + 1 = 4
shiyan = 1 * 3 + 1 = 4
���س���Ƶ����ߵ�3�������4��������ֵ�Ƶ�ʶ�Ϊ5���������Ƶ�ʶ�Ϊ3������ѡ���ȳ��ֵ���������"xinzeng"��"bendi" 
```


**��д�Ĵ��룺**(δ����汾�����ǱȽϺ���⣬���Լ򻯵ĵط���ʵ�ܶ�)

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

// ��allstrs�������ݷ�����
// ����Ĺ���������umap��vector �ıȽ�
void mysorted(vector<string> &allstrs, unordered_map<string, vector<vector<int>>>& umap){
    int n = allstrs.size();
    for(bool sorted = false; sorted = !sorted; n--){
        for(int i = 1; i < n; i++){

            ///////////�������,vector����������////////////
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
    //vector<vector<int>> [0]:��title�г��ֵ�λ�� [1]������
    //���磬�ںϲ���ı����д���"xinguan"�����ڵ�0��6�����ʵ�λ��
    ////   �ںϲ���������д���"xinguan"�����ڵ�4��21�����ʵ�λ��
    // ��ô��unordered_map�ж�Ӧ�ľ��ǣ�
    // it->first : "xinguan"
    // it->second : {{0, 6}, {4, 21}}
    string curstr;
    int i_title = 0;//�����г��ֵĵ���λ��
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
    int i_text = 0;//�����г��ֵĵ���λ��
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
    cout << "---�����������ǰ��allstrs---" << endl;
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
    cout << "---�������������allstrs---" << endl;
    for(auto str_tmp : allstrs){
        cout << str_tmp << " ";
    }
    cout << endl << endl;
    cout << "---test3 end----" << endl;
    /////////



    ///////test4
    cout << endl;
    cout << "---test4 start----" << endl;
    cout << "---��˳����������ʳ��ֵ�Ƶ��----" << endl;
    for(auto str_tmp: allstrs){
        cout << str_tmp << " = ";
        cout << umap[str_tmp][0].size() << " * 3 + ";
        cout << umap[str_tmp][1].size() << " = ";
        cout << umap[str_tmp][0].size() * 3 + umap[str_tmp][1].size() << endl;

    }
    cout << "---test4 end----" << endl;

    ///////////


    //�����ǰn�����
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


    //�������е�title��text��title_s��text_s
    string title_s[M];
    string text_s[M];
    int i = 0;
    while(i < M){
        getline(cin, title_s[i]);
        getline(cin, text_s[i]);
        i++;
    }

    // ��title_s��text_s�е������ַ����ֱ�ϳ�һ��title��text�ַ���
    ////��ʵ����ط����ǿ��Ըĳ�ֱ�Ӷ���һ���ַ�����
    //////////���߽�title_s��text_s��Ϊ����killQuestion1�����룬������û��
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
---�����������ǰ��allstrs---
xiaoguo sanqi wuzhong zhengti yiqing liangli lianghao yimiao tongguo yili xinzeng ju shenzhen bendi feiyan baodao anli chengdu linchuang kongzhi quezhen xinguan shiyan 

---test2 end----

---test3 start----
---�������������allstrs---
xinguan xinzeng bendi anli quezhen yimiao feiyan linchuang shiyan lianghao xiaoguo sanqi wuzhong zhengti yiqing liangli tongguo yili ju shenzhen baodao chengdu kongzhi 

---test3 end----

---test4 start----
---��˳����������ʳ��ֵ�Ƶ��----
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