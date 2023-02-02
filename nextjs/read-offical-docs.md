# NEXT.JS 공식문서 읽기
- [NEXT.JS 공식문서 튜토리얼](https://nextjs.org/learn/basics/create-nextjs-app)을 보며 정리한 문서입니다.
- 의역과 오역이 많을 수 있습니다. <br />
제가 보기 좋게 간단하게 정리할 예정입니다.

## **라우팅**

Next.js 에서는 폴더와 파일에 따라 페이지 경로가 결정됩니다.

- pages/index.js 는 / (루트) 와 연결되어있습니다.
- pages/posts/first-post.js 는 /posts/first-post 라우트와 연결되어 있습니다.

라우팅이 굉장히 쉽죠.

리액트 라우터에 비해 굉장히 쉽고 직관적이라 좋은 것 같습니다.

### **Link 컴포넌트**
~~~ javascript
<h1 className="title">
  Welcome to <a href="https://nextjs.org">Next.js!</a>
</h1>
~~~

HTML 태그인 a 태그가 아닌, Link를 쓰게 됩니다.

~~~ javascript
import Link from 'next/link';

<h1 className="title">
  Read <Link href="/posts/first-post">this page!</Link>
</h1>
~~~

Link 컴포넌트는 **client-side navigation** 을 할 수 있게 합니다. 자바스크립트를 통한 화면 전환으로 기존 a 태그를 통한 네비게이션보다 더 빠른 속도를 제공합니다.

적당히 정리해보자면, Link 컴포넌트를 사용함으로, 성능을 최적화 시킨다는 것 입니다.

이 부분에 대해 더 궁금해서 찾아봤습니다.

- Next.js APP에 요청이 있으면 html 과 기타 파일들이 다운로드됩니다.
- 이 페이지에 link 컴포넌트가 있다면, Next.js 에서 prefetching 을 합니다.

**prefetch 에 관한 내용은 후술하겠습니다.**

이후에도 재미있는 컴포넌트들이 많이 나옵니다.

- Imgae 컴포넌트
- Head 컴포넌트

기존 Html 코드들을 Next Js 에 맞게 재설계하고 최적화 시킨 컴포넌트로 보입니다. 자세한 공부는 후에 하겠습니다.

## **CSS Styling**

Next.js 에서는 CSS Module 을 지원합니다.

이전에 Layout Componet 에 대해 배웁니다.

### **Layout Component**

아래에 쓴 children 문법은 [JSX](https://beta.reactjs.org/learn/passing-props-to-a-component) 문법입니다.

~~~ javascript
export default function Layout({ children }) {
  return <div>{children}</div>;
}
~~~

Layout 컴포넌트로 원하는 컴포넌트를 덮습니다.

~~~ javascript
import Head from 'next/head';
import Link from 'next/link';
import Layout from '../../components/layout';

export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">← Back to home</Link>
      </h2>
    </Layout>
  );
}
~~~

이렇게 하면 공통적으로 쓰는 Layout을 더 쉽게 재사용할 수 있습니다.

### **Adding CSS**

> CSS Modules 을 사용하기 위해 CSS 파일명은 필히 .module.css 가 붙어야 합니다.

~~~ css
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
~~~

~~~ javascript
import styles from './layout.module.css';

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>;
}
~~~

위 처럼 class 명으로 지정하면 됩니다.

> 여기서 주목해야할 점은, 자동적으로 유니크한 클래스명을 가지게 된다는 부분입니다.

그렇기 때문에, CSS Modules 을 사용하면 컴포넌트끼리 클래스명이 겹치는 것을 걱정하지 않아도 됩니다.

하지만 하나하나 import 해야하고, 여러 클래스를 사용할 경우 처리하기 귀찮을 수 있습니다.

### **styled-jsx**

그런 단점 때문에 styled-JSX 을 사용하시는 분들도 있었습니다. ex) Nomad Corder

[styled-JSX 공식문서](https://github.com/vercel/styled-jsx)

[camille 님 velog](https://velog.io/@ka0son/NextJs)

내용이 너무 방대할 것 같기에 후에 따로 정리 해보도록 하겠습니다.

### **Global CSS**

[pages/_app.js](https://nextjs.org/docs/advanced-features/custom-app) 를 추가해서 Global CSS 를 추가할 수 있습니다.

App 컴포넌트는 initialize pages 입니다.

- 페이지가 바뀌어도 레이아웃을 유지합니다.
- 페이지가 바뀔 때에도 state 를 유지합니다.
- data를 페이지에 주입할 수 있습니다.
- global css 를 추가할 수 있습니다.

위와 같은 역할을 합니다.

~~~ javascript
import '../styles/global.css';

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
~~~

이런 식으로 추가할 수 있습니다.

> global CSS 파일은 오직 pages/_app.js 에서만 import 할 수 있습니다. 다른 곳에는 **할 수 없습니다.**

