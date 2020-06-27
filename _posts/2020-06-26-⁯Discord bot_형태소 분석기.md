---
layout: post
title: "Discord bot 형태소 분석기"
description: "python으로 내 감정을 알려주는 디스코드 봇을 만들어보자."
date: 2020-06-26
tags: [디스코드, 형태소 분석기, python]
comments: true
share: true

---

이 글은 저번 글 [How to make discord bot](https://dbrudals.github.io/2020-06-26/How-to-make-discord-bot/){: target=" _ blank"}에 이어서<br>형태소 분석기로 내 감정을 알려주는 봇을 만들어보겠습니다.<br>
봇을 아직 만들지 않으셨다면 위에 링크를 눌러 만들고 오세요~

--- 

## STEP 1. 빠밤
먼저, [감정사전](https://raw.githubusercontent.com/park1200656/KnuSentiLex/master/data/SentiWord_info.json)을 DB에 담아준다.<br>
아래 코드를 참고 해서 본인이 사용하고 계신 것과 맞게, 본인 스타일대로 하면 됩니다.<br>
감정사전에 polarity는 기분이 좋고, 나쁨을 나타낸다. 숫자가 크면 클수록 기분이 좋음을 나타낸다.
```
import pymysql.cursors
import json
import requests
from konlpy.tag import Kkma 

kkma = Kkma()

conn = pymysql.connect(host='dev-swh.ga',
                       user="root",
                       password="swhacademy!",
                       db="k_min",
                       charset='utf8')

response2 = requests.get('https://raw.githubusercontent.com/park1200656/KnuSentiLex/master/data/SentiWord_info.json')
data = json.loads(response2.text)

for i in data:
    a = kkma.pos(b[0])
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

```
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


## STEP 2. 빠밤

python에서 아래 코드를 기본으로 코딩하겠습니다.
```
import discord

client = discord.Client()

# 디스코드에서 생성된 토큰을 여기에 추가
token = ""

# 아래는 봇이 구동되었을 때 동작하는 부분
@client.event
async def on_ready():
    print("Logged in as ")  # 봇의 아이디, 닉네임이 출력
    print(client.user.name)
    print(client.user.id)


# 봇이 새로운 메시지를 수신했을때 동작하는 부분
@client.event
async def on_message(message):
    print(message)  # 로그 성 출력

    if message.author.bot:  # 봇이 메세지를 보냈다면..
        return None  # 걍 무시.
    if message.content.startswith('!안녕'):  # 만약 해당 메시지가 '!안녕' 으로 시작하는 경우에는
        # await client.send_message(channel, '안녕') #봇은 해당 채널에 '안녕' 이 라고 말합니다.
        await message.channel.send('안녕')

client.run(token)
```
token부분에는 How to make discord bot에 고이 모셔두라고 했던 그것을 넣으시면 됩니다.

## STEP 3.
이제 코드를 한부분 한부분 작성해나가면 됩니다.<br>
```
with conn.cursor() as cursor:
    sql = "select test, polarity from discord_emotion"
    cursor.execute(sql)
    result = cursor.fetchall()
```
위 코드로 DB에 담겨있는 형태소 분석한것을 가져옵니다.<br>
<br>
이제 내가 메세지를 보냈을 때 반응하는 코드를 짜주겠습니다.

```
if message.author.bot:  # 봇이 메세지를 보냈다면..
        return None  # 걍 무시.
```
위 코드는 봇이 여러 개일때 반응하는 것을 막기 위해 작성한 코드입니다.
<br>
내가 보낸 말을 형태소 분석한다.
  ```
  if message.content.startswith('!안녕'): # 만약 해당 메시지가 '!안녕' 으로 시작하는 경우에는
        await message.channel.send('안녕')
    pos = kkma.pos(message.content)
    text = ''
    for i in range(len(pos)):
        text += pos[i][0] + '+' + pos[i][1] + '/'
  ```
  
내가 보낸 메시지와 하나하나 비교하여 맞는 것을 찾은 뒤 숫자에 맞는 기분을 출력한다.
  
  ```
      for b in result:
        if text.startswith(b[0]): # 만약 해당 메시지가 ~~으로 시작하는 경우에는
            if b[1] == '-2':
                await message.channel.send('기분이 많이 좋지 않네요')
            elif b[1] == '-1':
                await message.channel.send('기분이 좋지 않네요')
            elif b[1] == '0':
                await message.channel.send('기분이 나쁘진 않네요')
            elif b[1] == '1':
                await message.channel.send('기분이 좋네요')
            elif b[1] == '2':
                await message.channel.send('기분이 많이 좋네요')
            break
```


## 마지막
그래서 완성한 코드는
```
import discord
import pymysql.cursors
from konlpy.tag import Kkma

client = discord.Client()


# 디스코드에서 생성된 토큰을 여기에 추가
token = ""

kkma = Kkma()

# 아래는 봇이 구동되었을 때 동작하는 부분
@client.event
async def on_ready():
    print("Logged in as ")  # 봇의 아이디, 닉네임이 출력
    print("")
    print("")

with conn.cursor() as cursor:
    sql = "select test, polarity from discord_emotion"
    cursor.execute(sql)
    result = cursor.fetchall()

# 봇이 새로운 메시지를 수신했을때 동작하는 부분
@client.event
async def on_message(message):
    print(message)  # 로그 성 출력
    text = ''
    if message.author.bot:  # 봇이 메세지를 보냈다면..
        return None  # 걍 무시.
    if message.content.startswith('!안녕'): # 만약 해당 메시지가 '!안녕' 으로 시작하는 경우에는
        await message.channel.send('안녕')
    pos = kkma.pos(message.content)
    for i in range(len(pos)):
        text += pos[i][0] + '+' + pos[i][1] + '/'
    for b in result:
        if text.startswith(b[0]): # 만약 해당 메시지가 ~~으로 시작하는 경우에는
            if b[1] == '-2':
                await message.channel.send('기분이 많이 좋지 않네요')
            elif b[1] == '-1':
                await message.channel.send('기분이 좋지 않네요')
            elif b[1] == '0':
                await message.channel.send('기분이 나쁘진 않네요')
            elif b[1] == '1':
                await message.channel.send('기분이 좋네요')
            elif b[1] == '2':
                await message.channel.send('기분이 많이 좋네요')
            break
            # await client.send_message(channel, '안녕') #봇은 해당 채널에 '안녕' 이 라고 말합니다.

client.run(token)

conn.close()
```
참고해서 본인만의 스타일로 다시 코드를 작성해도 좋을 것 같습니다.


--- 

이 문서는 [한글 Lorem Ipsum](http://guny.kr/stuff/klorem/)으로 생성되었습니다.
