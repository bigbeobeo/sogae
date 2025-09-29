<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Simple Matching App — 시작</title>
  <style>
    :root{
      --bg:#f5f7fb; --card:#ffffff; --accent:#4f46e5; --muted:#6b7280;
      --radius:12px; font-family: Inter, "Noto Sans KR", system-ui, -apple-system, "Segoe UI", Roboto;
    }
    *{box-sizing:border-box}
    body{
      margin:0; min-height:100vh; background:linear-gradient(180deg,#eef2ff 0%,var(--bg) 100%);
      display:flex; align-items:center; justify-content:center; padding:32px;
    }
    .app{
      width:100%; max-width:980px; background:none;
      display:grid; grid-template-columns: 420px 1fr; gap:24px;
    }

    /* 좌측: 프로필 폼 카드 */
    .card{
      background:var(--card); padding:20px; border-radius:var(--radius); box-shadow:0 6px 18px rgba(15,23,42,0.06);
    }
    h1{margin:0 0 8px; font-size:20px}
    p.lead{margin:0 0 16px; color:var(--muted); font-size:13px}

    label{display:block; font-weight:600; margin-top:10px; font-size:13px; color:#111827}
    input[type="text"], input[type="date"], textarea, select {
      width:100%; padding:10px 12px; margin-top:8px; border-radius:10px; border:1px solid #e6e9ef;
      background:#fbfdff; font-size:14px;
    }
    textarea{min-height:92px; resize:vertical; padding-top:10px}
    .row{display:flex; gap:12px}
    .small{flex:0 0 110px}
    .photo-preview{
      width:120px; height:120px; border-radius:12px; background:#eef2ff; display:flex;
      align-items:center; justify-content:center; overflow:hidden; border:1px dashed #dbeafe;
      margin-top:8px;
    }
    .photo-preview img{width:100%; height:100%; object-fit:cover;}
    .btn{
      display:inline-block; padding:10px 14px; border-radius:10px; border:0; cursor:pointer;
      background:var(--accent); color:#fff; font-weight:700; margin-top:14px;
    }
    .btn.secondary{background:#fff; color:var(--accent); border:1px solid #e6e9ef}
    .center{display:flex; align-items:center; gap:10px; flex-wrap:wrap}

    /* 우측: 탐색 / 결과 영역 */
    .panel{background:var(--card); padding:20px; border-radius:var(--radius); box-shadow:0 6px 18px rgba(15,23,42,0.04);}
    .gender-select{display:flex; gap:12px; margin-bottom:18px}
    .gender-select button{flex:1; padding:10px 12px; border-radius:10px; border:1px solid #eef2ff; cursor:pointer; font-weight:700}
    .gender-select button.active{background:var(--accent); color:#fff; border-color:transparent}
    .cards{display:grid; grid-template-columns: repeat(auto-fill,minmax(220px,1fr)); gap:12px}
    .user-card{background:linear-gradient(180deg,#fff 0%,#fbfdff 100%); padding:12px; border-radius:12px; border:1px solid #f1f5f9}
    .user-card .meta{display:flex; gap:10px; align-items:center}
    .user-card img{width:56px; height:56px; border-radius:10px; object-fit:cover}
    .user-card h3{margin:4px 0 0; font-size:15px}
    .user-card p{margin:6px 0 0; color:var(--muted); font-size:13px}
    .tag{display:inline-block; margin-top:8px; padding:6px 8px; border-radius:999px; font-size:12px; background:#eef2ff; color:#0f172a}

    .small-muted{font-size:12px; color:var(--muted)}
    .spaced{margin-top:10px}
    footer{margin-top:12px; text-align:right; color:var(--muted); font-size:12px}
    @media (max-width:900px){
      .app{grid-template-columns:1fr; padding:12px}
    }
  </style>
</head>
<body>
  <div class="app">
    <!-- 왼쪽: 프로필 입력 -->
    <div class="card" id="profileCard">
      <h1>시작 화면 — 내 정보 입력</h1>
      <p class="lead">이 앱은 데모용입니다. 이름·생년월일·나이·사진·특징을 입력한 뒤 등록하세요.</p>

      <label for="name">이름</label>
      <input id="name" type="text" placeholder="예: 김안덕" />

      <label for="birth">생년월일</label>
      <input id="birth" type="date" />

      <div class="row">
        <div class="small">
          <label for="age">나이</label>
          <input id="age" type="text" placeholder="자동 계산됨" readonly />
        </div>
        <div style="flex:1">
          <label for="gender">성별</label>
          <select id="gender">
            <option value="male">남성</option>
            <option value="female">여성</option>
            <option value="other">기타</option>
          </select>
        </div>
      </div>

      <label>얼굴 사진</label>
      <div class="center">
        <div class="photo-preview" id="photoPreview">미리보기</div>
        <div style="flex:1">
          <input id="photoInput" type="file" accept="image/*" />
          <div class="small-muted spaced">사진은 브라우저에만 저장됩니다. (로컬 저장)</div>
        </div>
      </div>

      <label for="desc">특징 / 소개 (간단히)</label>
      <textarea id="desc" placeholder="예: 취미는 등산, 치킨 좋아함, 대화 잘 통하는 사람 찾음"></textarea>

      <div style="display:flex; gap:10px; margin-top:8px;">
        <button class="btn" id="registerBtn">등록</button>
        <button class="btn secondary" id="clearBtn">초기화</button>
      </div>

      <div id="afterRegister" style="display:none; margin-top:14px;">
        <div class="small-muted">등록이 완료되었습니다. 오른쪽에서 상대 성별을 선택하세요.</div>
      </div>

      <footer>데모 앱 • 저장은 로컬에서만 유지됩니다.</footer>
    </div>

    <!-- 오른쪽: 상대 목록 / 탐색 -->
    <div class="panel">
      <h1 style="margin:0 0 8px">상대 탐색</h1>
      <p class="lead">원하는 성별 버튼을 눌러 다른 사용자를 확인하세요.</p>

      <div class="gender-select" id="genderBtns">
        <button data-g="male" id="maleBtn">남성 보기</button>
        <button data-g="female" id="femaleBtn">여성 보기</button>
      </div>

      <div style="display:flex; gap:12px; align-items:center; margin-bottom:12px;">
        <div class="small-muted">내 정보 요약</div>
        <div style="flex:1"></div>
        <button class="btn secondary" id="editBtn" style="padding:8px 10px">수정하기</button>
      </div>

      <div class="card" id="mySummary" style="margin-bottom:12px;">
        <div style="display:flex; gap:12px; align-items:center;">
          <div style="width:72px; height:72px; border-radius:10px; overflow:hidden; background:#f8fafc;" id="myImgWrap">
            <img id="myImg" src="" alt="" style="width:100%; height:100%; object-fit:cover; display:none" />
            <div id="noImg" style="padding:12px; color:var(--muted); font-size:13px">사진 없음</div>
          </div>
          <div style="flex:1">
            <div id="myName" style="font-weight:800"></div>
            <div id="myAgeGender" class="small-muted spaced"></div>
            <div id="myDesc" class="small-muted spaced"></div>
          </div>
        </div>
      </div>

      <div id="results">
        <div class="small-muted" id="resultsInfo">성별을 선택하면 목록이 나옵니다.</div>
        <div class="cards" id="cardsContainer" style="margin-top:12px"></div>
      </div>
    </div>
  </div>

  <script>
    // -------------------------
    // 유틸 / 초기 샘플 데이터
    // -------------------------
    const sampleUsers = [
      {id:1, name:'이준호', gender:'male', birth:'1996-03-12', desc:'축구 좋아하고 요리 가능', img:'https://images.unsplash.com/photo-1607746882042-944635dfe10e?q=80&w=800&auto=format&fit=crop&crop=faces'},
      {id:2, name:'박성민', gender:'male', birth:'1998-07-01', desc:'책과 영화 덕후', img:'https://images.unsplash.com/photo-1545996124-1b6d1b0b7f7d?q=80&w=800&auto=format&fit=crop&crop=faces'},
      {id:3, name:'최하늘', gender:'female', birth:'2000-11-20', desc:'사진 찍는 것 좋아함', img:'https://images.unsplash.com/photo-1544005313-94ddf0286df2?q=80&w=800&auto=format&fit=crop&crop=faces'},
      {id:4, name:'김지우', gender:'female', birth:'1999-05-06', desc:'등산 자주함, 커피덕후', img:'https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=800&auto=format&fit=crop&crop=faces'},
      {id:5, name:'한민우', gender:'male', birth:'1995-09-30', desc:'프로그래밍 / 홈카페', img:'https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?q=80&w=800&auto=format&fit=crop&crop=faces'}
    ];

    // -------------------------
    // DOM 참조
    // -------------------------
    const nameEl = document.getElementById('name');
    const birthEl = document.getElementById('birth');
    const ageEl = document.getElementById('age');
    const genderEl = document.getElementById('gender');
    const descEl = document.getElementById('desc');
    const photoInput = document.getElementById('photoInput');
    const photoPreview = document.getElementById('photoPreview');
    const registerBtn = document.getElementById('registerBtn');
    const clearBtn = document.getElementById('clearBtn');
    const afterRegister = document.getElementById('afterRegister');

    const maleBtn = document.getElementById('maleBtn');
    const femaleBtn = document.getElementById('femaleBtn');
    const cardsContainer = document.getElementById('cardsContainer');
    const resultsInfo = document.getElementById('resultsInfo');

    const myName = document.getElementById('myName');
    const myAgeGender = document.getElementById('myAgeGender');
    const myDesc = document.getElementById('myDesc');
    const myImg = document.getElementById('myImg');
    const noImg = document.getElementById('noImg');

    const editBtn = document.getElementById('editBtn');

    // -------------------------
    // HELPER: 나이 계산 (오늘 기준)
    // step-by-step: 연-월-일 비교로 생일 지났는지 판단
    // -------------------------
    function calculateAgeFromBirth(birthISO){
      if(!birthISO) return '';
      const b = new Date(birthISO);
      const today = new Date();
      let age = today.getFullYear() - b.getFullYear();
      // 생일이 아직 안지난 경우 -1
      const m = today.getMonth() - b.getMonth();
      if (m < 0 || (m === 0 && today.getDate() < b.getDate())) age--;
      return age;
    }

    // -------------------------
    // 사진 업로드 -> 미리보기(Base64 저장)
    // -------------------------
    let currentPhotoData = null;
    photoInput.addEventListener('change', (e)=>{
      const f = e.target.files && e.target.files[0];
      if(!f) return;
      const reader = new FileReader();
      reader.onload = function(ev){
        currentPhotoData = ev.target.result; // base64
        photoPreview.innerHTML = '<img src="'+currentPhotoData+'" alt="preview" />';
      };
      reader.readAsDataURL(f);
    });

    // 자동으로 나이 필드 업데이트
    birthEl.addEventListener('change', ()=>{
      const age = calculateAgeFromBirth(birthEl.value);
      ageEl.value = age === '' ? '' : age + '세';
    });

    // -------------------------
    // 저장 / 불러오기 (localStorage)
    // -------------------------
    const STORAGE_KEY = 'simple_matching_my_profile_v1';

    function saveMyProfile(profile){
      localStorage.setItem(STORAGE_KEY, JSON.stringify(profile));
    }
    function loadMyProfile(){
      const raw = localStorage.getItem(STORAGE_KEY);
      if(!raw) return null;
      try { return JSON.parse(raw); } catch(e){ return null; }
    }
    function clearMyProfile(){
      localStorage.removeItem(STORAGE_KEY);
    }

    // 화면에 내 정보 표시
    function renderMySummary(){
      const p = loadMyProfile();
      if(!p){
        myName.textContent = '등록된 정보가 없습니다.';
        myAgeGender.textContent = '';
        myDesc.textContent = '';
        myImg.style.display = 'none';
        noImg.style.display = 'block';
        return;
      }
      myName.textContent = p.name;
      myAgeGender.textContent = (p.age ? p.age+'세 • ' : '') + (p.gender || '');
      myDesc.textContent = p.desc || '';
      if(p.photo){
        myImg.src = p.photo;
        myImg.style.display = 'block';
        noImg.style.display = 'none';
      } else {
        myImg.style.display = 'none';
        noImg.style.display = 'block';
      }
    }

    // -------------------------
    // 등록 버튼
    // - 필수: 이름, 생년월일
    // - 나이 자동 계산
    // -------------------------
    registerBtn.addEventListener('click', ()=>{
      const name = nameEl.value.trim();
      const birth = birthEl.value;
      if(!name){ alert('이름을 입력하세요.'); nameEl.focus(); return; }
      if(!birth){ alert('생년월일을 선택하세요.'); birthEl.focus(); return; }

      const ageNum = calculateAgeFromBirth(birth);
      const gender = genderEl.value;
      const desc = descEl.value.trim();

      const profile = {
        name, birth, age: ageNum, gender, desc, photo: currentPhotoData
      };
      saveMyProfile(profile);

      afterRegister.style.display = 'block';
      renderMySummary();
      resultsInfo.textContent = '성별을 선택해 다른 사용자를 확인하세요.';
    });

    // 초기화 버튼
    clearBtn.addEventListener('click', ()=>{
      if(!confirm('정말 초기화 하시겠어요? (저장된 내 정보도 삭제됩니다)')) return;
      nameEl.value=''; birthEl.value=''; ageEl.value=''; genderEl.value='male'; descEl.value='';
      photoPreview.innerHTML = '미리보기'; currentPhotoData = null;
      clearMyProfile();
      afterRegister.style.display='none';
      renderMySummary();
      resultsInfo.textContent = '성별을 선택하면 목록이 나옵니다.';
      cardsContainer.innerHTML = '';
    });

    // 편집 버튼: 폼에 현재 저장값 불러오기
    editBtn.addEventListener('click', ()=>{
      const p = loadMyProfile();
      if(!p){ alert('먼저 정보를 등록하세요.'); return; }
      nameEl.value = p.name || '';
      birthEl.value = p.birth || '';
      ageEl.value = (p.age != null ? p.age + '세' : '');
      genderEl.value = p.gender || 'male';
      descEl.value = p.desc || '';
      if(p.photo){
        currentPhotoData = p.photo;
        photoPreview.innerHTML = '<img src="'+p.photo+'" />';
      } else {
        photoPreview.innerHTML = '미리보기';
        currentPhotoData = null;
      }
      window.scrollTo({top:0, behavior:'smooth'});
    });

    // -------------------------
    // 상대 목록 렌더링
    // - 샘플 데이터 사용 (실제 서비스면 서버 호출)
    // -------------------------
    function renderUsersForGender(g){
      cardsContainer.innerHTML = '';
      const matching = sampleUsers.filter(u => u.gender === g);
      if(matching.length === 0){
        resultsInfo.textContent = '해당 성별의 사용자가 없습니다.';
        return;
      }
      resultsInfo.textContent = `${matching.length}명 발견됨`;
      matching.forEach(u=>{
        const card = document.createElement('div');
        card.className = 'user-card';
        const age = calculateAgeFromBirth(u.birth);
        card.innerHTML = `
          <div class="meta">
            <img src="${u.img}" alt="${u.name}" />
            <div style="flex:1">
              <h3>${u.name} <span style="font-weight:600; color:#6b7280; font-size:12px">• ${age ? age+'세' : ''}</span></h3>
              <p class="small-muted">${u.desc}</p>
              <div class="tag">${u.gender === 'male' ? '남성' : '여성'}</div>
            </div>
          </div>
        `;
        cardsContainer.appendChild(card);
      });
    }

    // 버튼 이벤트
    maleBtn.addEventListener('click', ()=>{
      maleBtn.classList.add('active'); femaleBtn.classList.remove('active');
      renderUsersForGender('male');
    });
    femaleBtn.addEventListener('click', ()=>{
      femaleBtn.classList.add('active'); maleBtn.classList.remove('active');
      renderUsersForGender('female');
    });

    // -------------------------
    // 초기 로드: 내 정보 있으면 표시
    // -------------------------
    (function init(){
      const p = loadMyProfile();
      if(p){
        afterRegister.style.display = 'block';
        // summary 표시
        renderMySummary();
      } else {
        renderMySummary();
      }
    })();
  </script>
</body>
</html>
