#encoding=utf-8
'''
Created on 2016年4月18日
@author: lenovo
'''
import jieba;
import os;
class spamEmailBayes:
    #获得停用词表,即在这个表中的单词无效，不做考虑
    def getStopWords(self):
        stopList=[]
        for line in open(r"C:\Users\Lenovo-Y430p\Documents\Python Scripts\贝叶斯和输入法\贝叶斯统计\data\中文停用词表.txt"):
            stopList.append(line[:len(line)-1])
        return stopList;
    #获得词典
    def get_word_list(self,content,wordsList,stopList):
        #分词结果放入res_list
        res_list = list(jieba.cut(content))
        for i in res_list:
            if i not in stopList and i.strip()!='' and i!=None:
                if i not in wordsList:#每个文件只计算一个单词
                    wordsList.append(i)
                    
    #若列表中的词已在词典中，则加1，否则添加进去
    def addToDict(self,wordsList,wordsDict):
        for item in wordsList:
           wordsDict[item]=wordsDict.get(item,0)+1
           
           
    def get_File_List(self,filePath):
        filenames=os.listdir(filePath)
        return filenames
    
    #通过计算每个文件中p(s|w)来得到对分类影响最大的15个词
    def getTestWords(self,testDict,spamDict,normDict,normFilelen,spamFilelen):
        wordProbList={}
        for word,num  in testDict.items():
            if word in spamDict.keys() and word in normDict.keys():
                #该文件中包含词个数（分多种情况来讨论）
                #除以总的文件数（我关心的更多的是，是不是垃圾邮件，因此求得是比例）
                #对于单个分词不存在的我们假象为为0.01即比较低，对于都不存在的为了保证学习能力估计值为0.4
                pw_s=spamDict[word]/spamFilelen
                pw_n=normDict[word]/normFilelen
                ps_w=pw_s/(pw_s+pw_n) 
                wordProbList.setdefault(word,ps_w)
            if word in spamDict.keys() and word not in normDict.keys():
                pw_s=spamDict[word]/spamFilelen
                pw_n=0.01
                ps_w=pw_s/(pw_s+pw_n) 
                wordProbList.setdefault(word,ps_w)
            if word not in spamDict.keys() and word in normDict.keys():
                pw_s=0.01
                pw_n=normDict[word]/normFilelen
                ps_w=pw_s/(pw_s+pw_n) 
                wordProbList.setdefault(word,ps_w)
            if word not in spamDict.keys() and word not in normDict.keys():
                #若该词不在词典中，概率设为0.4
                wordProbList.setdefault(word,0.4)
        wordProbList1=sorted(wordProbList.items(),key=lambda d:d[1],reverse=True)
        return (dict(wordProbList1))
    
    #计算贝叶斯概率起名欠妥当
    def calBayes(self,wordList,spamdict,normdict):
        ps_w=1
        ps_n=1
         
        for word,prob in wordList.items() :
            print(word+"/"+str(prob))
            ps_w*=(prob)
            ps_n*=(1-prob)
        p=ps_w/(ps_w+ps_n)
#         print(str(ps_w)+"////"+str(ps_n))
        return p        

    #计算预测结果正确率
    def calAccuracy(self,testResult):
        rightCount=0
        errorCount=0
        for name ,catagory in testResult.items():
            if (int(name)<1000 and catagory==0) or(int(name)>1000 and catagory==1):
                rightCount+=1
            else:
                errorCount+=1
        return rightCount/(rightCount+errorCount)
