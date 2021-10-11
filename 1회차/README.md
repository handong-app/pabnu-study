# 유정섭 - 메일트래커

많은 자료들을 보고 검색을 해본 결과 Mailtracker 은 사진을 이용하는 방식을 이용하는 것 같다.

# 기존 서비스 조사

많이 사용되는 [mailtrack.io](http://mailtrack.io) 를 사용하여 예를 들자면,

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled.png)

mail track 을 사용한 메일이다. 일단 여기 있는 내용을 검사하자면,

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%201.png)

맨 끝에 아주 작은 이미지가 첨부 되어 있는 것을 볼 수 있었다.

더욱 자세히 보기 위해 메일 원본을 확인해 봤을 때,

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%202.png)

메일 끝에 이 링크가 들어가 있는 것을 알 수 있었고, 

[https://mailtrack.io/trace/mail/73b71a1e88306b7d3fcd2e25af754e88cb68fe76.png?u=7453972](https://mailtrack.io/trace/mail/73b71a1e88306b7d3fcd2e25af754e88cb68fe76.png?u=7453972)

해당 주소에 들어가본 결과 - 

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%203.png)

1x1 이미지가 있는 것을 볼 수 있었다.

### 참고한 문서

- [https://www.quora.com/Can-mailtrack-recipients-tell-their-email-has-been-tracked](https://www.quora.com/Can-mailtrack-recipients-tell-their-email-has-been-tracked)
- [https://www.quora.com/How-does-MailTrack-count-how-many-times-an-email-has-been-opened](https://www.quora.com/How-does-MailTrack-count-how-many-times-an-email-has-been-opened)
- [https://www.quora.com/How-does-email-tracking-opened-click-delete-work-in-general](https://www.quora.com/How-does-email-tracking-opened-click-delete-work-in-general)
- [https://stackoverflow.com/questions/822862/is-there-any-way-to-track-whether-an-email-has-been-opened](https://stackoverflow.com/questions/822862/is-there-any-way-to-track-whether-an-email-has-been-opened)

# 간단한 테스트

그러면 유저가 메일을 열어볼때만 이미지가 로딩되면서 서버에 요청을 보내는걸까?

파이썬에서는 간단한 웹 서버를 만들 수 있다.

```
python3 -m http.server 7800
```

이 방법을 이용해서 테스트를 해보았다.

## 방화벽 설정

주의: 끝나고 다시 막아두기

```
[jungle@li1548-239 data]$ sudo firewall-cmd --zone=public --add-port=7800/tcp
success
[jungle@li1548-239 data]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: dhcpv6-client http https ssh
  ports: 7800/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules:
```

막아 둘때는 그냥 reload 만 해주면 됨

```
sudo firewall-cmd --reload
```

## 테스트 진행

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%204.png)

보다시피 방금 만든 이미지 파일이다

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%205.png)

여기 보면 알다시피 로딩 할때마다 기록이 남는 것을 볼 수 있다.

### 시도 1 - 실패

그렇다는 말은 이 이미지를 사용해 메일을 보내보겠다.

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%206.png)

처음 메일을 보냈더니 총 3개의 요청이 생긴 것을 알 수 있다.

조금 있다가 메일을 열어보겠다.

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%207.png)

아무일도 없었다. 캐쉬가 남아서 그런것 같다.

### 시도 2

구글에서 몇번 해도 안되니 네이버로 가봤다..

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%208.png)

전송하자 마자 구글에서 이미지를 한번 긁어 간 것을 알 수 있다.

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%209.png)

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%2010.png)

아직 읽지 않음..

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%2011.png)

메일을 열었더니 요청이 새로 온 것을 확인할 수 있었다.

![Untitled](%E1%84%8B%E1%85%B2%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%89%E1%85%A5%E1%86%B8%20-%20%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%90%E1%85%B3%E1%84%85%E1%85%A2%E1%84%8F%E1%85%A5%20a8a705e5b97a41f5a96d18b618e3cd7b/Untitled%2012.png)

메일 원본 보기를 했는데 기본적으로 네이버도 내장시켜 보낸 다는 것을 알 수 있었다.

[네이버 고객센터](https://help.naver.com/support/contents/contents.help?serviceNo=2342&categoryNo=18917&lang=ko)