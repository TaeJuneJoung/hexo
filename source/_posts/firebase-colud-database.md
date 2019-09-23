---
title: Firebase Colud Database
date: 2019-08-08 23:34:32
tags: ['복합색인', 'Cloud Database']
categories: ['Firebase']
---

# Firebase Cloud Database_복합색인

cloud database에서 사용자의 id를 기반으로 데이터를 가져오고자 하였다.

초기 소스는 아래와 같이 진행되었다.

```js
getTodos() {
    const todosCollection = firestore.collection(TODOS);//+
    return todosCollection
      .orderBy("sequence", "asc")
      .get()
      .then(docSnapshots => {
        return docSnapshots.docs.map(doc => {
          let data = doc.data();
          data.created_at = new Date(data.created_at.toDate());
          data.id = doc.id;
          return data;
        });
      });
  }
```

이 결과는 다른 사람의 데이터까지 가져왔다.

개발을 모르는 사용자가 보았을 때는 아무 상관 없을 수도 있으나, 데이터가 많아진다면 다른 사람들의 데이터도 불러오기에 사용자 브라우저에 영향을 미칠 것이었다.

가져오는 데이터베이스에서 처리해야함을 깨닷고 찾아본 결과 `where`을 사용하면 되었다.

처음에는 구글링으로 example을 보면은 변수로 된 값을 비교한다기보단 string의 값 2개를 비교하여 잘 이해가 되지 않았는데, 자세히 보니 앞에는 사용하는 데이터 변수이름이었고 뒤에는 해당 값이었다.

`.where("uid", "==", this.store.state.user.email);`을 사용하여 데이터의 유저정보와 

현재 로그인 상태의 유저를 비교하여 해당 값만 가져오게 설계할 수 있었다.



설계에 1부분만 완성한다면 이제 원하는 값을 만날 수 있다.

console을 보면 처음 보는 오류가 있다. 그래도 친절하게 주소를 주어 해결방법으로 넘어가게 해준다.



```bash
Uncaught (in promise) FirebaseError: The query requires an index. You can create it here: https://console.firebase.google.com/project/project_id/database/firestore/indexes?create_composite=...
    at new FirestoreError (http://localhost:8080/app.js:2098:28)
    at JsonProtoSerializer../node_modules/@firebase/firestore/dist/index.cjs.js.JsonProtoSerializer.fromRpcStatus (http://localhost:8080/app.js:7608:16)
```

해당 `url`로 넘어가면 아래 사진과 같이 나오며, 진행해주면 문제가 해결된다.


{% asset_img slug firebase_cloud_error01.PNG %}


### 참고사이트

https://firebase.google.com/docs/firestore/query-data/queries?hl=ko#simple_queries