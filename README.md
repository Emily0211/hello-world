import requests, json
url = 'https://trends.google.com/trends/api/dailytrends'
payload = {
    "hl":"zh-TW",
    "tz":"-480",
    "ed":"20210823",
    "geo":"TW",
    "ns":"15",
}

html = requests.get(url, params = payload)
html.encoding = 'utf-8'
_,datas = html.text.split(',',1)
jsondata = json.loads(datas) #將下載資料轉換為字典
trendingSearchesDays = jsondata['default']['trendingSearchesDays']

for trendingSearchesDay in trendingSearchesDays:
    news = ""  #news = 下載的新聞
    formattedDate = trendingSearchesDay['formattedDate']
    news += '日期:' + formattedDate +'\n\n'
    
    for data in trendingSearchesDay['trendingSearches']:
        news += "主題關鍵字:" + data['title']['query'] + '\n\n'
        for content in data['articles']:
            news += '標題:'+ content['title'] + '\n'
            news += '媒體:'+  content['source']+ '\n'
            news += '發布時間:'+  content['timeAgo']+ '\n'
            news += '內容:'+  content['snippet']+ '\n'
            news += '相關連結:'+ content['url']+ '\n\n'
            
token = 'eNCoCNRBNPB8fvDuje5x61SkUhN9L79pu0myfPfD7hX'
def send_message(token, news):
    headers = {
        "Authorization": "Bearer " + token, 
        "Content-Type": "application/x-www-form-urlencoded"
    }
    params = {"message": news}
    requests.post("https://notify-api.line.me/api/notify", headers=headers, params=params)

if __name__ == '__main__':
    news = news
    token = ''
    send_message(token=token, news=news)
    
