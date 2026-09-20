# Vite 개발 서버 포트 고정하기

## 증상

도메인 제한이 걸린 API 키를 쓰는 프로젝트에서, 개발 서버를 다시 띄울 때마다 포트가 5173 에서 5174, 5175 로 밀린다. API 콘솔에는 5173 만 등록해 뒀으므로 요청이 전부 거부된다.

거부 메시지가 API 쪽에서 오기 때문에 키가 만료됐거나 코드가 잘못된 줄 알고 엉뚱한 곳을 보게 된다.

## 원인

Vite 는 지정한 포트가 이미 사용 중이면 조용히 다음 빈 포트로 넘어간다. 이전 개발 서버가 안 죽고 남아 있으면 이 일이 생긴다.

## 해결

```js
// vite.config.js
import { defineConfig } from 'vite'

export default defineConfig({
  server: {
    port: 5173,
    strictPort: true,
  },
})
```

`strictPort: true` 가 핵심이다. 포트를 못 잡으면 다른 포트로 넘어가는 대신 에러를 내고 멈춘다. 실패가 눈에 보이는 쪽이 낫다.

포트가 이미 물려 있으면 정리하고 다시 띄운다.

```bash
npx kill-port 5173
```

## 관련

[Discussions #1](https://github.com/sealworldking/Playground/discussions/1)
