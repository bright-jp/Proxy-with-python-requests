# Python Requestsでプロキシを使用する方法


[![Promo](https://github.com/bright-jp/Rotating-Residential-Proxies/blob/main/50%25%20off%20promo.png)](https://brightdata.jp/proxy-types/residential-proxies) 

このガイドでは、特に[Webスクレイピング](https://brightdata.jp/blog/how-tos/what-is-web-scraping)のために、Python requestsでプロキシを使用する方法を学びます。IPアドレスとロケーションを変更して、[Webサイトの制限を回避](https://brightdata.jp/blog/proxy-101/how-to-bypass-an-ip-ban)します。

- [Using a Proxy with a Python Request](#using-a-proxy-with-a-python-request)
- [Installing Packages](#installing-packages)
- [Components of Proxy IP Address](#components-of-proxy-ip-address)
- [Setting Proxies Directly in Requests](#setting-proxies-directly-in-requests)
- [Setting Proxies via Environment Variables](#setting-proxies-via-environment-variables)
- [Rotating Proxies Using a Custom Method and an Array of Proxies](#rotating-proxies-using-a-custom-method-and-an-array-of-proxies)
- [Using the Bright Data Proxy Service with Python](#using-the-bright-data-proxy-service-with-python)
- [Conclusion](#conclusion)

## Using a Proxy with a Python Request

## Installing Packages

`pip install` を使用して、Webページにリクエストを送信しリンクを収集するために、次のPythonパッケージをインストールします。

- `requests`: スクレイピングしたいデータがあるWebサイトにHTTPリクエストを送信します。
- `beautifulsoup4`: HTMLおよびXMLドキュメントを解析して、すべてのリンクを抽出します。

## Components of Proxy IP Address

プロキシサーバーの主要な構成要素は次の3つです。

1.  **Protocol** は通常、HTTPまたはHTTPSです。
2.  **Address** はIPアドレス、またはDNSホスト名です。
3.  **Port number** は0〜65535の範囲で、例: `2000` です。

したがって、プロキシIPアドレスは `https://192.167.0.1:2000` または
`https://proxyprovider.com:2000` のようになります。

## Setting Proxies Directly in Requests

このガイドでは、requestsでプロキシを設定する3つの方法を扱います。最初のアプローチは、requestsモジュールで直接設定することを想定しています。

次の手順で行います。

1. PythonスクリプトでRequestsとBeautiful Soupパッケージをインポートします。
2. プロキシサーバー情報を含む `proxies` というディレクトリを作成します。
3. `proxies` ディレクトリで、プロキシURLへのHTTPおよびHTTPS接続の両方を定義します。
4. データをスクレイピングしたいWebページのURLを設定するPython変数を定義します。`https://brightdata.jp` を使用します。
5. `request.get()` メソッドを使ってWebページにGETリクエストを送信します。引数はWebサイトのURLとproxiesの2つです。レスポンスは `response` 変数に格納されます。
6. `BeautifulSoup()` メソッドに `response.content` と `html.parser` を引数として渡し、リンクを収集します。
7. `find_all()` メソッドに `a` を引数として渡し、Webページ上のすべてのリンクを見つけます。
8. `get()` メソッドを使用して、各リンクの `href` 属性を抽出します。

以下が完全なソースコードです。

```
# import packages.  
import requests  
from bs4 import BeautifulSoup  
  
# Define proxies to use.  
proxies = {  
    'http': 'http://proxyprovider.com:2000',  
    'https': 'https://proxyprovider.com:2000',  
}  
  
# Define a link to the web page.  
url = "https://example.com/"   
  
# Send a GET request to the website.  
response = requests.get(url, proxies=proxies)  
  
# Use BeautifulSoup to parse the HTML content of the website.  
soup = BeautifulSoup(response.content, "html.parser")  
  
# Find all the links on the website.  
links = soup.find_all("a")  
  
# Print all the links.  
for link in links:  
    print(link.get("href"))
```

上記スクリプトを実行した際の出力は次のとおりです。

![Scraped links](https://github.com/bright-jp/Proxy-with-python-requests/blob/main/link-to-webpage-2-1024x653.png)

## Setting Proxies via Environment Variables

すべてのリクエストで同じプロキシを使用するには、ターミナルウィンドウで環境変数を設定するのが最適です。

```powershell
export HTTP_PROXY='http://proxyprovider.com:2000'  
export HTTPS_PROXY='https://proxyprovider.com:2000'
```

これで、スクリプトからproxies定義を削除できます。

```
# import packages.  
import requests  
from bs4 import BeautifulSoup  
  
# Define a link to the web page.  
url = "https://example.com/"   
  
# Send a GET request to the website.  
response = requests.get(url)  
  
# Use BeautifulSoup to parse the HTML content of the website.  
soup = BeautifulSoup(response.content, "html.parser")  
  
# Find all the links on the website.  
links = soup.find_all("a")  
  
# Print all the links.  
for link in links:  
    print(link.get("href"))
```

## Rotating Proxies Using a Custom Method and an Array of Proxies

[![Promo](https://github.com/bright-jp/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.jp/proxy-types/residential-proxies) 

ローテーティングプロキシは、同一IPアドレスから大量のリクエストを受け取った際にWebサイトが課す制限を回避するのに役立ちます。

次の手順で行います。

1. 次のPythonライブラリをインポートします: Requests、Beautiful Soup、Random。
2. ローテーション処理中に使用するプロキシのリストを作成します。`http://proxyserver.com:port` 形式を使用します。

```
# List of proxies  
proxies = [  
    "http://proxyprovider1.com:2010", "http://proxyprovider1.com:2020",  
    "http://proxyprovider1.com:2030", "http://proxyprovider2.com:2040",  
    "http://proxyprovider2.com:2050", "http://proxyprovider2.com:2060",  
    "http://proxyprovider3.com:2070", "http://proxyprovider3.com:2080",  
    "http://proxyprovider3.com:2090"  
]
```

3. `get_proxy()` というカスタムメソッドを作成します。これは `random.choice()` メソッドを使用してプロキシリストからランダムにプロキシを選択し、選択したプロキシを辞書形式（HTTPとHTTPSの両方のキー）で返します。新しいリクエストを送信するたびに、このメソッドを使用します。

```
# Custom method to rotate proxies  
def get_proxy():  
    # Choose a random proxy from the list  
    proxy = random.choice(proxies)  
    # Return a dictionary with the proxy for both http and https protocols  
    return {'http': proxy, 'https': proxy}  
```

4. ローテーションしたプロキシを使って、一定回数のGETリクエストを送信するループを作成します。各リクエストで `get()` メソッドは、`get_proxy()` メソッドで指定されたランダムに選ばれたプロキシを使用します。

5. 以前説明したとおり、Beautiful Soupパッケージを使用してWebページのHTMLコンテンツからリンクを収集します。

6. リクエスト処理中に発生した例外をキャッチして出力します。

この例の完全なソースコードは次のとおりです。

```
# import packages  
import requests  
from bs4 import BeautifulSoup  
import random  
  
# List of proxies  
proxies = [  
    "http://proxyprovider1.com:2010", "http://proxyprovider1.com:2020",  
    "http://proxyprovider1.com:2030", "http://proxyprovider2.com:2040",  
    "http://proxyprovider2.com:2050", "http://proxyprovider2.com:2060",  
    "http://proxyprovider3.com:2070", "http://proxyprovider3.com:2080",  
    "http://proxyprovider3.com:2090"  
]

# Custom method to rotate proxies  
def get_proxy():  
    # Choose a random proxy from the list  
    proxy = random.choice(proxies)  
    # Return a dictionary with the proxy for both http and https protocols  
    return {'http': proxy, 'https': proxy}  
  
  
# Send requests using rotated proxies  
for i in range(10):  
    # Set the URL to scrape  
    url = 'https://brightdata.jp/'  
    try:  
        # Send a GET request with a randomly chosen proxy  
        response = requests.get(url, proxies=get_proxy())  
  
        # Use BeautifulSoup to parse the HTML content of the website.  
        soup = BeautifulSoup(response.content, "html.parser")  
  
        # Find all the links on the website.  
        links = soup.find_all("a")  
  
        # Print all the links.  
        for link in links:  
            print(link.get("href"))  
    except requests.exceptions.RequestException as e:  
        # Handle any exceptions that may occur during the request  
        print(e)
```

## Using the Bright Data Proxy Service with Python

Bright Dataには、7,200万以上の[レジデンシャルプロキシIP](https://brightdata.jp/proxy-types/residential-proxies)と、770,000以上の[データセンタープロキシ](https://brightdata.jp/proxy-types/datacenter-proxies)からなる大規模なネットワークがあります。

[Bright Dataのデータセンタープロキシを統合](https://brightdata.jp/integration)して、Python requestsに組み込むことができます。Bright Dataのアカウントを作成したら、次の手順で最初のプロキシを作成します。

1. ようこそページで **View proxy product** をクリックし、Bright Dataが提供するさまざまな種類のプロキシを確認します。

![Bright Data proxy types](https://github.com/bright-jp/Proxy-with-python-requests/blob/main/bright-data-proxy-types-1024x464.png)

2. **Datacenter Proxies** を選択して新しいプロキシを作成し、次のページで詳細を追加して保存します。

![Datacenter proxies configuration](https://github.com/bright-jp/Proxy-with-python-requests/blob/main/datacenter-proxies-837x1024.png)

3. プロキシが作成されると、ダッシュボードにhost、port、username、passwordなど、スクリプトで使用するパラメータが表示されます。

![Datacenter proxy parameters](https://github.com/bright-jp/Proxy-with-python-requests/blob/main/datacenter-proxy-parameters-928x1024.png)

4. これらのパラメータをスクリプトにコピー＆ペーストし、プロキシURLは `username-(session-id)-password@host:port` の形式を使用します。

> **Note:**\
> `session-id` は、`random` というPythonパッケージを使用して作成されるランダムな数値です。

以下は、PythonリクエストでBright Dataのプロキシを使用するコードです。

```
import requests  
from bs4 import BeautifulSoup  
import random  
  
# Define parameters provided by Brightdata  
host = 'brd.superproxy.io'  
port = 33335  
username = 'username'  
password = 'password'  
session_id = random.random()  
  
# format your proxy  
proxy_url = ('http://{}-session-{}:{}@{}:{}'.format(username, session_id,  
                                                     password, host, port))  
  
# define your proxies in dictionary  
proxies = {'http': proxy_url, 'https': proxy_url}  
  
# Send a GET request to the website  
url = "https://example.com/"   
response = requests.get(url, proxies=proxies)  
  
# Use BeautifulSoup to parse the HTML content of the website  
soup = BeautifulSoup(response.content, "html.parser")  
  
# Find all the links on the website  
links = soup.find_all("a")  
  
# Print all the links  
for link in links:  
    print(link.get("href"))
```

このコードを実行すると、Bright Dataの[プロキシサービス](https://brightdata.jp/proxy-types)を使用して正常にリクエストが行われます。

## Conclusion

Bright DataのWebプラットフォームを使用すると、世界中のあらゆる国や都市をカバーする、プロジェクト向けの信頼性の高いプロキシを入手できます。今すぐBright Dataの[proxy services](https://brightdata.jp/proxy-types)を無料でお試しください。