'''
https://yoonpunk.tistory.com/6

#**조선일보, 카테고리,페이지수입력후 기사 full내용 크롤링하는 함수**
#remain : html.parser 대신 lxml
#********real final 2019-03-06*********
#[catogory]
#1경제             7전국
#2정치             8스포츠
#3사회             9연예
#4국제
#5문화
#6 오피니언
#'''
import sys
from bs4 import BeautifulSoup
import urllib.request
from urllib.parse import quote


def get_text(URL, output_file):
    source_code_from_url = urllib.request.urlopen(URL)
    # urlllib로 기사 페이지를 요청받습니다.
    soup = BeautifulSoup(source_code_from_url, 'html.parser')
    soup.encoding = 'utf-8' 
    # BeautifulSoup로 페이지를 분석하기위해 soup변수로 할당 받습니다.
    content_of_article = soup.select('#news_body_id > div')
    # 기사의 본문내용을 추출
    for item in content_of_article:
        string_item = str(item.find_all(text=True))
        output_file.write(string_item)                          


def crawlling(page,category):
    page = int(page)
    
    for page in range(1,page+1):
        URL_with_page_num = 'http://news.chosun.com/svc/list_in/list.html?catid='+str(category)+'&pn='+str(page)
        source_code_from_URL = urllib.request.urlopen(URL_with_page_num)
        soup = BeautifulSoup(source_code_from_URL, 'html.parser')
        soup.encoding = 'utf-8' 


        i = 0
        output_file_name = 'C:/feb/{category}_page{page}.txt'.format(category=category,page=page) 
        output_file = open(output_file_name, 'w', -1,"utf-8")  # for문 전에 

        for title in soup.find_all('dt'):
            title_link = title.select('a')
            article_URL = title_link[0]['href']
            get_text(article_URL, output_file)
            i +=1

        output_file.close()                                        # for문 밖에서 (shift+tab)


