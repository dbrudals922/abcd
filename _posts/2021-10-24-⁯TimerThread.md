---
layout: post
title: "[Python]감정을 알려주는 Discord bot 만들기"
description: "python으로 내 감정을 알려주는 디스코드 봇을 만들어보자."
date: 2021-10-24
tags: [java, quartz]
comments: true
share: true

---

[How to make discord bot](https://dbrudals.github.io/2020-06-26/How-to-make-discord-bot/){: target=" _ blank"}에 이어서<br>형태소 분석기를 이용해 내 감정을 알려주는 디스코드 봇을 만드는 글입니다.<br>

--- 

## STEP 1. 빠밤
먼저, [감정사전](https://raw.githubusercontent.com/park1200656/KnuSentiLex/master/data/SentiWord_info.json)을 DB에 담아준다.<br>
아래 코드를 참고 해서 본인이 사용하고 계신 것과 맞게, 본인 스타일대로 하면 됩니다.<br>
감정사전에 polarity는 기분이 좋고, 나쁨을 나타낸다. 숫자가 크면 클수록 기분이 좋음을 나타낸다.

```python
import pymysql.cursors
import json
import requests
from konlpy.tag import Kkma

kkma = Kkma()


conn = pymysql.connect(host='',
                       user="",
                       password="",
                       db="",
                       charset='utf8')

response2 = requests.get('https://raw.githubusercontent.com/park1200656/KnuSentiLex/master/data/SentiWord_info.json')
data = json.loads(response2.text)

for i in data:
    a = kkma.pos(i["word"])
    text = ''
    for j in range(len(a)):
        text += a[j][0] + '+' + a[j][1] + '/'
    with conn.cursor() as cursor:
        sql = 'INSERT INTO discord_emotion (word, word_root, polarity, test) VALUES (%s, %s, %s, %s)'
        cursor.execute(sql, (i["word"], i["word_root"], i["polarity"], text))
    conn.commit()

conn.close()
```

저는 [kkma 형태소 분석기](http://kkma.snu.ac.kr/documents/){: target="_ blank"}를 사용 하였습니다.
자세한 내용은 들어가보셔서 확인하시면 좋을 거 같습니다.

```python
    a = kkma.pos(b[0])
    text = ''
    for j in range(len(a)):
        text += a[j][0] + '+' + a[j][1] + '/'
```

위 코드가 형태소 분석을 하는 코드입니다..<br>
ex)가격+NNG/이+JKS/싸+VV/다+EFN/<br>
저는 예시와 같이 만들어 DB에 넣어주겠습니다.<br>
그 후 DB 에는
![emotion DB](/images/emotion db.jpg){: width="500"}
이런식으로 들어갑니다. DB 코드 부분은 본인이 사용하고 계신 걸로 작성하셔서 하시면 될것같습니다~


--- 
