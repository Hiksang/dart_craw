# dart_craw
opendartreader를 활용하여 거래소 상장기업(코스피, 코스닥, 코넥스)의 사업보고서를 다운받을 수 있는 소스코드.


## env
python3.10.13
~~~bash

pip install -r requriements.txt

~~~

## 사용 방법
~~~python

for index, row in kospi.iterrows():
    try:
        ComapanyName = row['회사명']
        CompanyCode = row['종목코드']
        print(row['회사명'], row['종목코드'])
        
        os.makedirs(f'./report/kospi/{ComapanyName}', exist_ok=True)
        
        df = dart.list(CompanyCode, start='2023-01-01',kind='A') # start를 변경하여 다개년도 사업보고서 추출 가능.
        for rcp_no in df['rcept_no']:
        
            files = dart.attach_files(rcp_no)
            for title, url in files.items():
                print(title)
                print(url)
                dart.download(url,f'./report/kospi/{ComapanyName}/{title}')
        print(index)
    except:
        pass
    sleep(1) ## api 호출 초과를 방지하기 위한 정지

~~~