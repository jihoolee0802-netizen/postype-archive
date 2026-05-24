<!DOCTYPE html>
<html lang="ko">
<head>

<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>

<title>POSTYPE ARCHIVE</title>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@400;500;600;700&display=swap" rel="stylesheet">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
-webkit-tap-highlight-color:transparent;
}

body{
background:#F7FAFD;
font-family:'Pretendard',sans-serif;
color:#111;
padding:18px 12px 120px;
}

.container{
width:100%;
max-width:700px;
margin:auto;
}

.header{
margin-bottom:22px;
}

.title{
font-size:30px;
font-weight:700;
letter-spacing:-0.03em;
}

.subtitle{
margin-top:8px;
font-size:14px;
color:#8DA8C2;
}

.search-wrap{
margin-bottom:18px;
}

.search{
width:100%;
height:52px;
border:none;
outline:none;
border-radius:18px;
padding:0 18px;
font-size:15px;
background:white;

box-shadow:
0 4px 18px rgba(158,196,232,0.08);
}

.feed{
display:flex;
flex-direction:column;
gap:14px;
}

.card{
background:white;
border-radius:24px;
padding:20px;

border:1px solid #EEF3F8;

box-shadow:
0 6px 20px rgba(158,196,232,0.08);

transition:0.18s ease;
}

.card:active{
transform:scale(0.985);
}

.top{
display:flex;
justify-content:space-between;
align-items:flex-start;
gap:12px;
}

.book-title{
font-size:20px;
font-weight:700;
line-height:1.4;
word-break:break-word;
}

.rating{
font-size:15px;
color:#9EC4E8;
flex-shrink:0;
margin-top:2px;
}

.tags{
display:flex;
flex-wrap:wrap;
gap:8px;
margin-top:14px;
}

.tag{
padding:7px 12px;
border-radius:999px;
background:#EDF5FC;
font-size:13px;
color:#5F8FBF;
font-weight:600;
}

.comment{
margin-top:16px;
font-size:15px;
line-height:1.7;
color:#555;
word-break:break-word;
}

.info{
margin-top:18px;
display:flex;
gap:8px;
flex-wrap:wrap;
}

.info-badge{
padding:7px 12px;
border-radius:999px;
background:#F4F6F8;
font-size:12px;
color:#666;
font-weight:600;
}

.buttons{
display:flex;
gap:10px;
margin-top:20px;
}

.btn{
flex:1;
height:46px;

border:none;
border-radius:16px;

font-size:15px;
font-weight:700;

cursor:pointer;
transition:0.18s ease;
}

.read-btn{
background:#9EC4E8;
color:white;
}

.open-btn{
background:#EEF4FA;
color:#5F8FBF;
}

.btn:active{
transform:scale(0.97);
}

.empty{
text-align:center;
padding:80px 20px;
color:#9AAABA;
font-size:15px;
}

footer{
margin-top:42px;
text-align:center;
font-size:13px;
color:#A8B8C7;
}

.add-button{
position:fixed;
bottom:24px;
left:50%;
transform:translateX(-50%);

width:calc(100% - 24px);
max-width:700px;

height:58px;

border:none;
border-radius:22px;

background:#9EC4E8;
color:white;

font-size:17px;
font-weight:700;

cursor:pointer;

box-shadow:
0 10px 30px rgba(158,196,232,0.35);

z-index:999;
}

.overlay{
position:fixed;
inset:0;

background:rgba(0,0,0,0.35);

display:none;
justify-content:center;
align-items:flex-end;

z-index:1000;
}

.editor{
width:100%;
max-width:700px;

background:white;

border-radius:30px 30px 0 0;

padding:24px 18px 40px;

max-height:92vh;
overflow:auto;
}

.editor-title{
font-size:26px;
font-weight:700;
margin-bottom:20px;
}

.input{
width:100%;

border:none;
outline:none;

background:#F5F7FA;

border-radius:18px;

padding:16px;
margin-bottom:14px;

font-size:15px;
font-family:'Pretendard',sans-serif;
}

textarea{
resize:none;
min-height:120px;
}

.submit{
width:100%;
height:54px;

border:none;
border-radius:18px;

background:#9EC4E8;
color:white;

font-size:16px;
font-weight:700;

cursor:pointer;
}

@media (max-width:480px){

.title{
font-size:26px;
}

.book-title{
font-size:18px;
}

.comment{
font-size:14px;
}

}

</style>

</head>
<body>

<div class="container">

<div class="header">

<div class="title">
POSTYPE ARCHIVE
</div>

<div class="subtitle">
my reading archive ✦
</div>

</div>

<div class="search-wrap">

<input
type="text"
class="search"
id="searchInput"
placeholder="작품 검색..."
>

</div>

<div class="feed" id="feed"></div>

<footer>
made with love ♡
</footer>

</div>

<button
class="add-button"
onclick="openEditor()"
>
+ 작품 추가
</button>

<!-- editor -->

<div class="overlay" id="overlay">

<div class="editor">

<div class="editor-title">
작품 추가
</div>

<input
class="input"
id="titleInput"
placeholder="작품 제목"
>

<input
class="input"
id="ratingInput"
type="number"
placeholder="별점 (1~5)"
>

<input
class="input"
id="tagsInput"
placeholder="태그 (쉼표로 구분)"
>

<input
class="input"
id="writerInput"
placeholder="작가 이름"
>

<input
class="input"
id="channelInput"
placeholder="채널 이름"
>

<input
class="input"
id="commentInput"
placeholder="한마디"
>

<input
class="input"
id="postypeInput"
placeholder="포스타입 링크"
>

<input
class="input"
id="notionInput"
placeholder="노션 링크"
>

<input
class="input"
id="archiveInput"
placeholder="저장공간 이름 (예: romance)"
>

<textarea
class="input"
id="memoInput"
placeholder="개인 메모 (선택)"
></textarea>

<button
class="submit"
onclick="addBook()"
>
저장하기
</button>

</div>

</div>

<script>

//////////////////////////////////////////////////
// firebase
//////////////////////////////////////////////////

const firebaseConfig = {

apiKey: "AIzaSyAqiZMWRoysVz49wsE6TEKeQ_M0W4MAeuo",
authDomain: "postype-archive-97f37.firebaseapp.com",
projectId: "postype-archive-97f37",
storageBucket: "postype-archive-97f37.firebasestorage.app",
messagingSenderId: "814038109995",
appId: "1:814038109995:web:adbcb330229a1f88733f6e",
measurementId: "G-58Z05345V2"

};

firebase.initializeApp(firebaseConfig);

const db = firebase.firestore();

//////////////////////////////////////////////////
// archive
//////////////////////////////////////////////////

const params =
new URLSearchParams(location.search);

const archiveId =
params.get('archive') || 'default';

//////////////////////////////////////////////////
// elements
//////////////////////////////////////////////////

const feed =
document.getElementById('feed');

const overlay =
document.getElementById('overlay');

const searchInput =
document.getElementById('searchInput');

let books = [];

//////////////////////////////////////////////////
// open / close
//////////////////////////////////////////////////

function openEditor(){

overlay.style.display = 'flex';

document.getElementById('archiveInput').value =
archiveId;

}

overlay.addEventListener('click',(e)=>{

if(e.target === overlay){

overlay.style.display = 'none';

}

});

//////////////////////////////////////////////////
// realtime sync
//////////////////////////////////////////////////

db.collection("archives")
.doc(archiveId)
.collection("books")
.orderBy("createdAt","desc")
.onSnapshot(snapshot=>{

books = snapshot.docs.map(doc=>({

id: doc.id,
...doc.data()

}));

renderBooks(books);

});

//////////////////////////////////////////////////
// render
//////////////////////////////////////////////////

function renderBooks(list){

feed.innerHTML = '';

if(list.length === 0){

feed.innerHTML = `

<div class="empty">
아직 저장된 작품이 없어 👀
</div>

`;

return;

}

list.forEach(book=>{

const card =
document.createElement('div');

card.className = 'card';

const stars =
'★'.repeat(book.rating || 0);

const tags =
(book.tags || [])
.map(tag=>`
<div class="tag">#${tag}</div>
`)
.join('');

card.innerHTML = `

<div class="top">

<div class="book-title">
${book.title || ''}
</div>

<div class="rating">
${stars}
</div>

</div>

<div class="tags">
${tags}
</div>

<div class="comment">
“${book.comment || ''}”
</div>

<div class="info">

<div class="info-badge">
${book.writer || '작가 미입력'}
</div>

<div class="info-badge">
${book.channel || '채널 미입력'}
</div>

</div>

<div class="buttons">

<button
class="btn read-btn"
onclick="window.open('${book.postype}','_blank')"
>
읽기
</button>

<button
class="btn open-btn"
onclick="window.location.href='${book.notion}'"
>
열기
</button>

</div>

`;

feed.appendChild(card);

});

}

//////////////////////////////////////////////////
// add
//////////////////////////////////////////////////

async function addBook(){

const title =
document.getElementById('titleInput').value;

const rating =
Number(document.getElementById('ratingInput').value);

const tags =
document
.getElementById('tagsInput')
.value
.split(',')
.map(tag=>tag.trim())
.filter(Boolean);

const writer =
document.getElementById('writerInput').value;

const channel =
document.getElementById('channelInput').value;

const comment =
document.getElementById('commentInput').value;

const postype =
document.getElementById('postypeInput').value;

const notion =
document.getElementById('notionInput').value;

const memo =
document.getElementById('memoInput').value;

const archive =
document.getElementById('archiveInput').value
|| 'default';

if(!title){

alert('제목 입력해줘!');

return;

}

await db
.collection("archives")
.doc(archive)
.collection("books")
.add({

title,
rating,
tags,
writer,
channel,
comment,
postype,
notion,
memo,

createdAt: Date.now()

});

overlay.style.display = 'none';

document
.querySelectorAll('.input')
.forEach(input=>{

input.value = '';

});

}

//////////////////////////////////////////////////
// search
//////////////////////////////////////////////////

searchInput.addEventListener('input',()=>{

const value =
searchInput.value
.toLowerCase()
.trim();

const filtered =
books.filter(book=>{

const target = `

${book.title}
${book.comment}
${book.writer}
${book.channel}
${(book.tags || []).join(' ')}

`.toLowerCase();

return target.includes(value);

});

renderBooks(filtered);

});

</script>

</body>
</html>
