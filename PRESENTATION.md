# DSAP24SUMMER

# 8/12
# Chapter09-1
# 파일 읽기 및 쓰기

# 읽기 모드 : r, 쓰기모드 w, 추가 모드 a, 텍스트 모드 t, 바이너리 모드 b
# 상대 경로('../, ./'), 절대 경로('C:\Django\example..')

# 파일 읽기(Read)
# 예제1

f = open('./resource/it_news.txt', 'r', encoding='UTF-8')
# 속성 확인
print(dir(f))
# 인코딩 확인
print(f.encoding)
# 파일 이름
print(f.name)
# 모드 확인
print(f.mode)
cts = f.read()
print(cts)
# 반드시 close
f.close()

# 예제2
with open('./resource/it_news.txt', 'r', encoding='UTF-8') as f:
    c = f.read()
    print(c)
    print(iter(c))
    print(list(c))

print()

# 예제3
# read() : 전체 읽기 , read(10) : 10Byte

with open('./resource/it_news.txt', 'r', encoding='UTF-8') as f:
    c = f.read(20)
    print(c)
    c = f.read(20)
    print(c)
    c = f.read(20)
    print(c)
    f.seek(0,0)
    c = f.read(20)
    print(c)

print()

# 예제4
# readline : 한 줄 씩 읽기

with open('./resource/it_news.txt', 'r', encoding='UTF-8') as f:
    line = f.readline()
    print(line)
    line = f.readline()
    print(line)


print()

# 예제5
# readlines : 전체를 읽은 후 라인 단위 리스트로 저장

with open('./resource/it_news.txt', 'r', encoding='UTF-8') as f:
    cts = f.readlines()
    print(cts)
    print()
    for c in cts:
        print(c, end='')
        
print()

# 파일 쓰기(write)

# 예제1
with open('./resource/contents1.txt', 'w') as f:
    f.write('I love python\n')

# 예제2
with open('./resource/contents1.txt', 'a') as f:
    f.write('I love python2\n')
    
    
# 예제3
# writelines : 리스트 -> 파일
with open('./resource/contents2.txt', 'w') as f:
    list = ['Orange\n', 'Apple\n', 'Banana\n', 'Melon\n']
    f.writelines(list)
    
# 예제4
with open('./resource/contents3.txt', 'w') as f:
    print('Test Text Write!', file=f)
    print('Test Text Write!', file=f)
    print('Test Text Write!', file=f)
# Chapter09-2
# CSV 파일 읽기 및 쓰기

# CSV : MIME - text/csv

import csv

# 예제1
with open('./resource/test1.csv', 'r') as f:
    reader = csv.reader(f)
    # next(reader) Header Skip
    # 객체 확인
    print(reader)
    # 타입 확인
    print(type(reader))
    # 속성 확인
    print(dir(reader))  # __iter__
    print()

    for c in reader:
        # print(c)
        # 타입 확인
        print(type(c))
        # list to str
        print(''.join(c))

# 예제2
with open('./resource/test1.csv', 'r') as f:
    reader = csv.reader(f, delimiter=',')  # 구분자 선택
    # next(reader) Header 스킵
    # 확인

    for c in reader:
        # print(c)
        print(''.join(c))

# # 예제3 (Dict 변환)
with open('./resource/test1.csv', 'r') as f:
    reader = csv.DictReader(f)
    # 확인
    print(reader)
    print(type(reader))
    print(dir(reader))  # __iter__ 확인
    print()

    for c in reader:
        for k, v in c.items():
            print(k, v)
        print('-----')

# 예제4
w = [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12], [13, 14, 15], [16, 17, 18], [19, 20, 21]]

with open('./resource/write1.csv', 'w', encoding='utf-8') as f:
    print(dir(csv))
    wt = csv.writer(f)
    # dir 확인
    print(dir(wt))
    # 타입 확인
    print(type(wt))
    for v in w:
        wt.writerow(v)

# 예제5
with open('./resource/write2.csv', 'w', newline='') as f:
    # 필드명
    fields = ['one', 'two', 'three']
    # Dict Writer 선언
    wt = csv.DictWriter(f, fieldnames=fields)
    # Herder Write
    wt.writeheader()

    for v in w:
        wt.writerow({'one': v[0], 'two': v[1], 'three': v[2]})

# 행맨 게임
# 시간 처리
import time
# 랜덤
import random
# csv 처리
import csv
# 사운드 처리
import winsound
#처음 인사
name = input("What is your name? ")

print("Hi!, " + name, "Time to play hangman game!")

print()

#1초 대기
time.sleep(1)

print("Start loading...")
print()

# 0.5초 대기
time.sleep(0.5)

# CSV 단어 리스트
words = []

# 문제 CSV 파일 로드
with open('./resource/word_list.csv', 'r') as f:
	reader = csv.reader(f)
	# Header Skip
	next(reader)
	for c in reader:
		words.append(c)

# 리스트 섞기
random.shuffle(words)
# 임의의 단어 선택
q = random.choice(words)

#정답 단어
word = q[0].strip()

#추측 단어
guesses = ''

#기회
turns = 10

# 핵심 While Loop
# 찬스 카운트가 남아 있을 경우
while turns > 0:
    # 실패 횟수
    failed = 0

    # 정답 단어 반복
    for char in word:
        # 정답 단어 내에 추측 단어가 포함되어 있는 경우
        if char in guesses:
            # 추측 단어 출력
            print (char, end=' ')
        else:
            # 틀린 경우 대시로 처리
            print ("_", end=' ')
            # 실패 횟수 증가
            failed += 1

    # 단어 추측이 성공한 경우
    if failed == 0:
        print()
        print()
		# 성공 사운드
        winsound.PlaySound('./sound/good.wav',winsound.SND_FILENAME)
		# 축하 메시지
        print("Congratulations! The Guesses is correct.")
        # while 구문 중단
        break

    print()
    # 추측 단어 글자 단위 입력
    print()
    print('Hint : {}'.format(q[1].strip()))
    guess = input("guess a character:")

    # 단어 더하기
    guesses += guess

    # 정답 단어에 추측한 문자가 포함되어 있지 않으면
    if guess not in word:
        # 기회 횟수 감소
        turns -= 1
        # 오류 메시지
        print("Oops! Wrong")
        # 남은 기회 출력
        print("You have", + turns, 'more guesses!')

        # 기회를 모두 사용하면
        if turns == 0:
			# 탈락 사운드
            winsound.PlaySound('./sound/bad.wav',winsound.SND_FILENAME)
            # 실패 메시지
            print("You hangman game failed. Bye!")
