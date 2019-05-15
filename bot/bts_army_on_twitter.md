---
layout: default
title: bts_army_on_twitter
---

## BTS 아미 트윗 🤖

방탄소년단 아미의 트윗을 모아주는 봇입니다.

- 개발자: skettee

- 깃허브 주소: [bts_army_on_twitter](https://github.com/skettee/bts_army_on_twitter)


### 개발 환경 만들기

봇을 개발하기 위해서는 몇가지 소프트웨어를 설치하고 환경을 설정해야 합니다.
[개발 환경 만들기](https://github.com/moabogey/docs/wiki/개발환경만들기)를 참조 하세요.


### 코드 실행

- 터미널 실행

  - 🖼 Windows PowerShell을 실행한다.

  - 🍎 Terminal을 실행한다.

- 작업할 폴더를 생성한다.

```
mkdir MyWork
```

- 작업할 폴더로 이동한다.

```
cd MyWork
```

- 깃 클론 (Git Clone)을 수행한다.

```
git clone https://github.com/skettee/bts_army_on_twitter.git
```

- 복사한 코드의 폴더로 이동한다.

```
cd bts_army_on_twitter
```

- VSCode를 실행한다.

```
code .
```

- 왼쪽 EXPLORE에서 `bas_army_on_twitter.py`를 선택한다.

- 하단 바에 `Python3.7.3 64-bit('base':conda)`를 누른다.

- `Python 3.6.8 64-bit ('moabogey':conda)`를 선택한다.

- 소스 코드에 `RunCell | Run Below`에서 `Run Below`를 누른다.

- 데이터가 정상적으로 수집이 되는지 오른쪽 Python Interactive에서 확인한다.


### 코드 분석

bts_army_on_twitter.py를 분석합니다.
봇의 소스 코드는 크게 세단계로 나눌 수 있습니다.

1. 사이트의 HTML에서 데이터를 수집

2. 포스트의 HTML에서 데이터를 수집

3. 데이터 저장


**사이트의 HTML에서 데이터를 수집**

- 데이터를 수집할 사이트의 정보와 주소를 설정합니다. 여기에서는 https://twitter.com/BTS_ARMY 에서 데이터를 수집합니다.

- requests와 beautifulsoup4를 이용해서 사이트의 HTML을 가져오고 파일로 저장합니다.

- 저장된 HTML파일 (twitter_source.html)을 열어 봅니다. 여기서 우리는 "포스트의 리스트"를 표현하는 구간을 찾을 것입니다. **포스트**는 제목, 내용, 이미지, 작성자, 작성 날짜 및 페이지 위치(URL)를 가지고 있는 하나의 문서(여기서는 트윗)를 나타내는 용어로 사용합니다.

```
+------------+ +-> <div class="content">
|   Post 1
|  (Tweet 1)
+------------+ +-> </div>

+------------+ +-> <div class="content">
|   Post 2
|  (Tweet 2)
+------------+ +-> </div>

+------------+ +-> <div class="content">
|   Post3
|  (Tweet 3)
+------------+ +-> </div>
```

- 각각의 포스트는 `<div class="content">` 에서 시작 되고 `</div>`로 끝난다는 것을 알아내는 것이 중요합니다. 이것은 사이트마다 다르기 때문에 이것을 찾아내는 것은 약간의 경험이 필요합니다.

- 발견한 포스트에서 아래와 같이 작성자, 올린 시간 및 포스트 위치(URL)을 찾습니다.

```
+-------------+
|   Post 1
+-------------+
|  createdBy  +-> <strong class="fullname...> </strong>
|
|  post URL   +-> <a class="tweet-timestamp... href=...>
|
|  createdAt  +-> <a class="tweet-timestamp... title=...>
|
+-------------+
```

- 나머지 데이터를 수집하기 위해서 포스트 HTML로 이동합니다.


**포스트의 HTML에서 데이터를 수집**

- 데이터를 수집할 포스트의 주소를 설정합니다.

- requests와 beautifulsoup4를 이용해서 사이트의 HTML을 가져오고 파일로 저장합니다.

- 저장된 HTML파일 (twitter_post_source.html)을 열어 봅니다. 여기서 우리는 포스트의 제목, 요약, 이미지, 사이트 이름, 포스트 주소(URL), 수집 날짜 및 시간 데이터를 수집할 것입니다.


**Open Graph Protocol**

- 대부분의 사이트들은 우리가 수집할 데이터를 사이트의 첫머리에 미리 모아 놓고 있습니다. 이 규약(Protocol)은 사이트를 모두 분석하지 않고도 사이트의 내용을 파악하는데 도움이 됩니다.

- 아래와 같은 메타 태그를 사용합니다.

```html
<head>
...
  <meta content="..." property="og:url"/>
  <meta content="..." property="og:title"/>
  <meta content="..." property="og:image"/>
  <meta content="..." property="og:description"/>
  <meta content="..." property="og:site_name"/>
...
</head>
```

- 메타 태그에서 데이터를 수집합니다.


**데이터 저장**

- 수집한 데이터를 선별해서 중복되는 것을 제외하고 데이터베이스에 저장합니다. 모아보기 봇은 하루에 24번 이상 동작 하도록 되어 있기 때문에 한번에 모든 데이터를 수집하지 않고 가장 최근의 데이터 1~2개를 수집하는 것이 원칙입니다. 여기서는 데이터를 수집한 날짜와 동일한 날에 등록된 포스트만 수집하도록 프로그래밍되어 있습니다.


### 참고 사이트

- [개발 환경 만들기](https://github.com/moabogey/docs/wiki/개발환경만들기)

- [예제 코드 실행](https://github.com/moabogey/docs/wiki/예제코드실행)

- [코딩을 하기 전에](https://github.com/moabogey/docs/wiki/코딩하기전에)

- [예제 코드 분석](https://github.com/moabogey/docs/wiki/예제코드분석)

- [봇 개발 하기](https://github.com/moabogey/docs/wiki/봇개발하기)


### ⬇️소스코드



```python
import requests
from requests.exceptions import HTTPError
from bs4 import BeautifulSoup
import re
import json
from datetime import datetime
from datetime import timedelta

if __debug__:
    import os.path

# 모아보기 컴포넌트를 가져온다.
import moabogey_database as moabogey
from moabogey_id import *

# 사이트 이름
site_name = 'twitter'
# 사이트에서 가져올 주제
subject_name = 'BTS_ARMY'
# 사이트 주소
site_url = 'https://twitter.com/BTS_ARMY'
if __debug__:
    print('🤟{}🤟 데이터 수집 중... ⚙️'.format(site_url))

# 사이트의 HTML을 가져온다.
try:
    response = requests.get(site_url)
    # 에러가 발생 했을 경우 에러 내용을 출력하고 종료한다.
    response.raise_for_status()
except HTTPError as http_err:
    print(f'HTTP error occurred: {http_err}')
except Exception as err:
    print(f'Other error occurred: {err}')
else:
    html_source = response.text
    
    # BeautifulSoup 오브젝트를 생성한다.
    soup = BeautifulSoup(html_source, 'html.parser')
    
    # HTML을 분석하기 위해서 페이지의 소스를 파일로 저장한다.
    if __debug__:
        file_name = site_name + '_source.html'
        if not os.path.isfile(file_name):
            print('file save: ', file_name)
            with open(file_name, 'w', encoding='utf-8') as f:
                f.write(soup.prettify())
       
    # 데이터를 저장할 데이터베이스를 생성한다. 
    # bot_id는 moabogey_id에서 가져온 값이다.
    db_name = subject_name + '_on_' + site_name 
    my_db = moabogey.Dbase(db_name, bot_id)
            
    # 반복해서 포스트의 목록을 하나씩 검색하며 데이터를 수집한다.
    for post in soup.find_all('div', class_='content'):
        # 포스트를 올린 작성자를 수집한다.
        moa_createBy = post.find('strong', class_='fullname').text.strip()
        #print('createdBy: ', moa_createBy)

        # 포스트의 주소를 수집한다.
        href = post.find('a', {"class": "tweet-timestamp", "href": True})
        if href:
            href = href['href'] 
            href_url = 'https://twitter.com' + href
            #print('href: ', href_url)

            # 포스트를 올린 날짜를 수집한다.
            if __debug__:
                # LANGUAGE KOREA CASE
                post_time = post.find('a', class_="tweet-timestamp")['title'].replace('오전', 'AM').replace('오후', 'PM')
                # 시간 형식 - AM 2:27 - 2019년 3월 11일
                moa_createdAt = datetime.strptime(post_time, '%p %I:%M - %Y년 %m월 %d일')
            else:
                # LANGUAGE UNDEFINED CASE
                post_time = post.find('a', class_="tweet-timestamp")['title']
                # 시간 형식 - 8:01 PM - 30 Mar 2019
                moa_createdAt = datetime.strptime(post_time, '%I:%M %p - %d %b %Y')

            # 시간을 보정한다. UTC (+7) ?
            moa_createdAt = moa_createdAt + timedelta(hours=7)

            # 포스트의 HTML을 가져온다.
            try:
                response =  requests.get(href_url)
                response.raise_for_status()
            except HTTPError as http_err:
                print(f'HTTP error occurred: {http_err}')
            except Exception as err:
                print(f'Other error occurred: {err}')
            else:
                subhtml_source = response.text
                # BeautifulSoup 오브젝트를 생성한다.
                post = BeautifulSoup(subhtml_source, 'html.parser')

                # HTML을 분석하기 위해서 포스트의 소스를 파일로 저장한다.
                if __debug__:
                    file_name = site_name + '_post_source.html'
                    if not os.path.isfile(file_name):
                        print('file save: ', file_name)
                        with open(file_name, 'w', encoding='utf-8') as f:
                            f.write(post.prettify())

                # 포스트 제목을 수집/가공 한다.
                #moa_title = post.find('meta', property="og:title")
                moa_title = post.find('meta', property="og:description")
                if moa_title:
                    moa_title = moa_title['content'].splitlines()[0]
                
                # 포스트 요약을 수집/가공한다.
                moa_desc = post.find('meta', property="og:description")
                if moa_desc:
                    moa_desc = ''.join(moa_desc['content'].splitlines()[1:])
                
                # 대표 이미지의 주소를 수집한다.
                moa_image = post.find('meta', property="og:image")
                if moa_image:
                    moa_image = moa_image['content']
                
                # 사이트 이름을 수집한다.
                moa_site_name = post.find('meta', property="og:site_name")
                if moa_site_name:
                    moa_site_name = moa_site_name['content']
                
                # 포스트의 주소를 수집한다.
                moa_url = href_url
                
                # 현재 날짜와 시간을 수집한다.
                moa_timeStamp = datetime.now()

                # 오늘 발행된 포스트만 선택한다.
                delta = moa_timeStamp - moa_createdAt + timedelta(hours=9)
                if delta.days <=0:
                
                    # 데이터베이스에 있는 포스트와 중복되는지를 확인한다.
                    if my_db.isNewItem('title', moa_title):
                        # 데이터 타입을 확인한다.
                        assert type(moa_title) == str, 'title: type error'
                        assert type(moa_desc) == str, 'desc: type error'
                        assert type(moa_url) == str, 'url: type error'
                        assert type(moa_image) == str, 'image: type error'
                        assert type(moa_site_name) == str, 'siteName: type error'
                        assert type(moa_createBy) == str, 'createBy: type error'
                        assert type(moa_createdAt) == datetime, 'createdAt: type error'
                        assert type(moa_timeStamp) == datetime, 'timeStamp: type error'
                        
                        # JSON형식으로 수집한 데이터를 변환한다.
                        db_data = { 'title': moa_title, 
                            'desc': moa_desc,
                            'url': moa_url,
                            'image': moa_image,
                            'siteName': moa_site_name,
                            'createdBy': moa_createBy,
                            'createdAt': moa_createdAt,
                            'timeStamp': moa_timeStamp
                        }

                        if __debug__:
                            # 디버그를 위해서 수집한 데이터를 출력한다.
                            temp_data = db_data.copy()
                            temp_data['desc'] = temp_data['desc'][:20] + '...'
                            print('📀 수집한 json data: ')
                            print(json.dumps(temp_data, indent=4, ensure_ascii=False, default=str))

                        # 수집한 데이터를 데이터베이스에 전송한다.
                        my_db.insertTable(db_data)

    # 데이터 베이스에 저장된 데이터를 디스플레이 한다.
    if __debug__:
        my_db.displayHTML()

    # 데이터 베이스를 닫는다.
    my_db.close()
    
```

    🤟https://twitter.com/BTS_ARMY🤟 데이터 수집 중... ⚙️
    📀 수집한 json data: 
    {
        "title": "“LOOK AT all the @BTS_twt fans that joined us this morning! ",
        "desc": "https://t.co/zTVJpSb...",
        "url": "https://twitter.com/GMA/status/1128673934268882944",
        "image": "https://pbs.twimg.com/amplify_video_thumb/1128673605162885121/img/4kvSNXez49TeWTwa.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 14:50:00",
        "timeStamp": "2019-05-16 00:54:34.728455"
    }
    📀 수집한 json data: 
    {
        "title": "“.@BTS_twt \"honored\" to tie Beatles’ record.",
        "desc": "\"We're all fanboys o...",
        "url": "https://twitter.com/ABC/status/1128659250496958464",
        "image": "https://pbs.twimg.com/media/D6nNmcvUwAEliMm.jpg",
        "siteName": "Twitter",
        "createdBy": "ABC News",
        "createdAt": "2019-05-15 13:51:00",
        "timeStamp": "2019-05-16 00:54:36.150537"
    }
    📀 수집한 json data: 
    {
        "title": "“FULL PERFORMANCE: @BTS_twt performs 'Boys With Luv' LIVE on @GMA in Central Park, kicking off our 2019 Summer Concert Series! https://t.co/zTVJpSbXPg",
        "desc": "#BTSonGMA 💜#BTS#ARMY...",
        "url": "https://twitter.com/GMA/status/1128665658781241347",
        "image": "https://pbs.twimg.com/media/D6nTbSyWkAApE9L.jpg:large",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 14:17:00",
        "timeStamp": "2019-05-16 00:54:37.135033"
    }
    📀 수집한 json data: 
    {
        "title": "“#BTS_TODAY 190515 The Late Show with Stephen Colbert #BTS @BTS_twt @bts_bighit @BigHitEnt @colbertlateshow #BTSonLSSC”",
        "desc": "...",
        "url": "https://twitter.com/BTS_ARMY/status/1128659298135855104",
        "image": "https://pbs.twimg.com/media/D6nNpbQUwAExLYo.jpg:large",
        "siteName": "Twitter",
        "createdBy": "BTS A.R.M.Y",
        "createdAt": "2019-05-15 13:52:00",
        "timeStamp": "2019-05-16 00:54:37.732589"
    }
    📀 수집한 json data: 
    {
        "title": "“Who’s ready for #BTS tonight? #BTSonLSSC ✨”",
        "desc": "...",
        "url": "https://twitter.com/colbertlateshow/status/1128658214717988864",
        "image": "https://pbs.twimg.com/media/D6nMlQTW4AAOxeS.jpg:large",
        "siteName": "Twitter",
        "createdBy": "The Late Show",
        "createdAt": "2019-05-15 13:47:00",
        "timeStamp": "2019-05-16 00:54:39.588269"
    }
    📀 수집한 json data: 
    {
        "title": "“[#오늘의방탄] Good Morning America Summer Concert☀️ with BTS😆",
        "desc": "#GMAwithBTS #BTSonGM...",
        "url": "https://twitter.com/bts_bighit/status/1128650754426675201",
        "image": "https://pbs.twimg.com/media/D6nF3bKVUAAvcCY.jpg:large",
        "siteName": "Twitter",
        "createdBy": "BTS_official",
        "createdAt": "2019-05-15 13:18:00",
        "timeStamp": "2019-05-16 00:54:40.793358"
    }
    📀 수집한 json data: 
    {
        "title": "“We’re touched by how much 💜 our cops are getting from the #BTSARMY this morning at the @GMA summer concert series at @CentralParkNYC. ",
        "desc": "At least we think it...",
        "url": "https://twitter.com/NYPDnews/status/1128645862287081472",
        "image": "https://pbs.twimg.com/ext_tw_video_thumb/1128645745081561088/pu/img/6b_2cwTcPj1Q3bNL.jpg",
        "siteName": "Twitter",
        "createdBy": "NYPD NEWS",
        "createdAt": "2019-05-15 12:58:00",
        "timeStamp": "2019-05-16 00:54:42.154092"
    }
    📀 수집한 json data: 
    {
        "title": "“Can it be #BTSonGMA every morning?! 💜",
        "desc": "#BTS#ARMY@bts_twt ht...",
        "url": "https://twitter.com/GMA/status/1128645343002877952",
        "image": "https://pbs.twimg.com/tweet_video_thumb/D6nA8EnUcAA-Aho.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:56:00",
        "timeStamp": "2019-05-16 00:54:43.185252"
    }
    📀 수집한 json data: 
    {
        "title": "“.@GMA PURPLES @BTS_TWT 💜",
        "desc": "Thank you #ARMY for ...",
        "url": "https://twitter.com/GMA/status/1128646086241816577",
        "image": "https://pbs.twimg.com/tweet_video_thumb/D6nBniuUUAATmoz.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:59:00",
        "timeStamp": "2019-05-16 00:54:44.321436"
    }
    📀 수집한 json data: 
    {
        "title": "“Errbody say la la la la la 🎶",
        "desc": "💜💜💜@BTS_twt#BTSonGMA...",
        "url": "https://twitter.com/GMA/status/1128644229742252032",
        "image": "https://pbs.twimg.com/media/D6m_8EAU0AIC2a7.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:52:00",
        "timeStamp": "2019-05-16 00:54:45.288092"
    }
    📀 수집한 json data: 
    {
        "title": "“MOVES ARE 🔥",
        "desc": "#BTSonGMA 💜#BTS#ARMY...",
        "url": "https://twitter.com/GMA/status/1128644963497943041",
        "image": "https://pbs.twimg.com/media/D6nAm2bV4AALXnG.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:55:00",
        "timeStamp": "2019-05-16 00:54:46.260336"
    }
    📀 수집한 json data: 
    {
        "title": "“Boy, are we in LUV with @BTS_TWT! 💜💜💜 #BoyWithLuv #BTSonGMA",
        "desc": "#BTS#ARMY https://t....",
        "url": "https://twitter.com/GMA/status/1128640694975774721",
        "image": "https://pbs.twimg.com/media/D6m8ucQU0AMGydw.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:38:00",
        "timeStamp": "2019-05-16 00:54:47.390153"
    }
    📀 수집한 json data: 
    {
        "title": "“.@Ginger_Zee is hanging with #ARMY 💜",
        "desc": "WE'RE SO CLOSE TO #B...",
        "url": "https://twitter.com/GMA/status/1128636165400064000",
        "image": "https://pbs.twimg.com/tweet_video_thumb/D6m4l3PUcAA6Ciw.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:20:00",
        "timeStamp": "2019-05-16 00:54:48.317956"
    }
    📀 수집한 json data: 
    {
        "title": "“WE  💜 IT! @BTS_twt is crushing our @KingsHawaiian party in Central Park this morning! #BTSonGMA https://t.co/NQ5ReXA6P5”",
        "desc": "...",
        "url": "https://twitter.com/GMA/status/1128640066430033920",
        "image": "https://pbs.twimg.com/media/D6m8JpwV4AUH0kc.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:35:00",
        "timeStamp": "2019-05-16 00:54:49.422920"
    }
    📀 수집한 json data: 
    {
        "title": "“😂 https://t.co/ongl3RAmVy”",
        "desc": "...",
        "url": "https://twitter.com/GMA/status/1128636962309648386",
        "image": "https://pbs.twimg.com/profile_images/1057986330129514497/ZZJdCMM2_400x400.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:23:00",
        "timeStamp": "2019-05-16 00:54:50.556741"
    }
    📀 수집한 json data: 
    {
        "title": "“#BTSonGMA is the #1 trend, WORLDWIDE on @Twitter!",
        "desc": "#BTSARMY, we are SO ...",
        "url": "https://twitter.com/GMA/status/1128638568090488838",
        "image": "https://pbs.twimg.com/amplify_video_thumb/1128638461475479553/img/B5o9IJ6g-vSsnnyQ.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:29:00",
        "timeStamp": "2019-05-16 00:54:51.530934"
    }
    📀 수집한 json data: 
    {
        "title": "“💜💜💜💜 #BTSonGMA @BTS_twt #BTSARMY ",
        "desc": "https://t.co/6chNiva...",
        "url": "https://twitter.com/GIPHY/status/1128639243109175296",
        "image": "https://pbs.twimg.com/tweet_video_thumb/D6m7OKrX4AI4okT.jpg",
        "siteName": "Twitter",
        "createdBy": "GIPHY",
        "createdAt": "2019-05-15 12:32:00",
        "timeStamp": "2019-05-16 00:54:52.720750"
    }
    📀 수집한 json data: 
    {
        "title": "“OH MY MY MY!",
        "desc": "#BTSonGMA ⁦@BTS_twt⁩...",
        "url": "https://twitter.com/THETonyMorrison/status/1128639747839098880",
        "image": "https://pbs.twimg.com/media/D6m7tm2XkAARtCh.jpg:large",
        "siteName": "Twitter",
        "createdBy": "Tony Morrison",
        "createdAt": "2019-05-15 12:34:00",
        "timeStamp": "2019-05-16 00:54:53.562919"
    }
    📀 수집한 json data: 
    {
        "title": "“GOOD MORNING AMERICA! WE 💜@bts_twt! #BTSonGMA https://t.co/5JzFk2ajAk”",
        "desc": "...",
        "url": "https://twitter.com/GMA/status/1128631142557601792",
        "image": "https://pbs.twimg.com/ext_tw_video_thumb/1128630865280622592/pu/img/hKcjxaePOA9aOIIW.jpg",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 12:00:00",
        "timeStamp": "2019-05-16 00:54:54.639030"
    }
    📀 수집한 json data: 
    {
        "title": "“RT if you're ready for #BTSonGMA!!!! 💜",
        "desc": "@BTS_twt #ARMY”...",
        "url": "https://twitter.com/GMA/status/1128630701497176070",
        "image": "https://pbs.twimg.com/media/D6mzgAGXsAMI6RX.jpg:large",
        "siteName": "Twitter",
        "createdBy": "Good Morning America",
        "createdAt": "2019-05-15 11:58:00",
        "timeStamp": "2019-05-16 00:54:55.806796"
    }




<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/amplify_video_thumb/1128673605162885121/img/4kvSNXez49TeWTwa.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“LOOK AT all the @BTS_twt fans that joined us this morning! </b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6nNmcvUwAEliMm.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“.@BTS_twt "honored" to tie Beatles’ record.</b></h4> 
    <hr>
    <p> By ABC News </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6nTbSyWkAApE9L.jpg:large" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“FULL PERFORMANCE: @BTS_twt performs 'Boys With Luv' LIVE on @GMA in Central Park, kicking off our 2019 Summer Concert Series! https://t.co/zTVJpSbXPg</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6nNpbQUwAExLYo.jpg:large" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“#BTS_TODAY 190515 The Late Show with Stephen Colbert #BTS @BTS_twt @bts_bighit @BigHitEnt @colbertlateshow #BTSonLSSC”</b></h4> 
    <hr>
    <p> By BTS A.R.M.Y </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6nMlQTW4AAOxeS.jpg:large" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“Who’s ready for #BTS tonight? #BTSonLSSC ✨”</b></h4> 
    <hr>
    <p> By The Late Show </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6nF3bKVUAAvcCY.jpg:large" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“[#오늘의방탄] Good Morning America Summer Concert☀️ with BTS😆</b></h4> 
    <hr>
    <p> By BTS_official </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/ext_tw_video_thumb/1128645745081561088/pu/img/6b_2cwTcPj1Q3bNL.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“We’re touched by how much 💜 our cops are getting from the #BTSARMY this morning at the @GMA summer concert series at @CentralParkNYC. </b></h4> 
    <hr>
    <p> By NYPD NEWS </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/tweet_video_thumb/D6nA8EnUcAA-Aho.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“Can it be #BTSonGMA every morning?! 💜</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/tweet_video_thumb/D6nBniuUUAATmoz.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“.@GMA PURPLES @BTS_TWT 💜</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6m_8EAU0AIC2a7.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“Errbody say la la la la la 🎶</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6nAm2bV4AALXnG.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“MOVES ARE 🔥</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6m8ucQU0AMGydw.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“Boy, are we in LUV with @BTS_TWT! 💜💜💜 #BoyWithLuv #BTSonGMA</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/tweet_video_thumb/D6m4l3PUcAA6Ciw.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“.@Ginger_Zee is hanging with #ARMY 💜</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6m8JpwV4AUH0kc.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“WE  💜 IT! @BTS_twt is crushing our @KingsHawaiian party in Central Park this morning! #BTSonGMA https://t.co/NQ5ReXA6P5”</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/profile_images/1057986330129514497/ZZJdCMM2_400x400.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“😂 https://t.co/ongl3RAmVy”</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/amplify_video_thumb/1128638461475479553/img/B5o9IJ6g-vSsnnyQ.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“#BTSonGMA is the #1 trend, WORLDWIDE on @Twitter!</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/tweet_video_thumb/D6m7OKrX4AI4okT.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“💜💜💜💜 #BTSonGMA @BTS_twt #BTSARMY </b></h4> 
    <hr>
    <p> By GIPHY </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6m7tm2XkAARtCh.jpg:large" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“OH MY MY MY!</b></h4> 
    <hr>
    <p> By Tony Morrison </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/ext_tw_video_thumb/1128630865280622592/pu/img/hKcjxaePOA9aOIIW.jpg" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“GOOD MORNING AMERICA! WE 💜@bts_twt! #BTSonGMA https://t.co/5JzFk2ajAk”</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>





<style>
  .card {
    width: 340px;
    margin-bottom: 20px;
    border: 1px solid #DDDDDD;
    border-radius: 4px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  .center {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
  }
  .card_item {
    width: 340px;
    padding: 5px 10px;
  }
  hr { 
    display: flex;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    margin-left: auto;
    margin-right: 100px;
    border-style: inset;
    border-width: 1px;
  } 
  h5 {
  	color: blue;
  }
</style>
<div class="card">
  <img src="https://pbs.twimg.com/media/D6mzgAGXsAMI6RX.jpg:large" alt="Image" class="center">
  <div class="card_item">
    <h5>Twitter</h5>
    <h4><b>“RT if you're ready for #BTSonGMA!!!! 💜</b></h4> 
    <hr>
    <p> By Good Morning America </p>
  </div>
</div>


