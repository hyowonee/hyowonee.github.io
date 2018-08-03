---
title: github blog에 youtube video 삽입하기
author: Choi Hyowon
---
* Today's Quotes 애플리케이션 개발 프로젝트를 진행하는 와중에 MainView의 grid를 적용하는 과정이 있었다.
* 블로그에 이미지와 글로써 올리려 해봤으나 이해가 힘든점이 많아서 동영상으로 업로드 하기로 결정하였다.
* 찾아보니 markdown으로는 iframe 삽입이 되지 않아서 우회하는 방식으로 동영상을 업로드 하였다.

1. blog `_includes` 폴더에 `constraintExample.html`이라는 업로드를 위한 html 파일을 만든다.
2. `constraintExample.html`안에 다음과 같은 코드를 삽입해준다.

```html
<iframe width="730" height="500" src="https://www.youtube.com/embed/id" frameborder="0" allowfullscreen></iframe>
```
너비와 높이는 자유롭게 설정한다. 처음에 id 자리를 비워놔서 동영상 재생에 에러가 생겼다. 
자신의 유튜브 동영상 링크를 보면 `https://www.youtube.com/watch?v=C3NqbKZk_g8` 이런식으로 되어있는데, `watch?v=` 뒤의 것이 id가 된다.

3. 동영상을 삽입할 자리에 `{% include constraintExample.html id="C3NqbKZk_g8" %}` 코드를 삽입한다. 물론 id는 자신의 동영상 id를 사용해야 한다.

## 결과
![Image](/images/youtube_video.png)
