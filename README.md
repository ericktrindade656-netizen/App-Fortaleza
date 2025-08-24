// ===================== CONSTANTES =====================
const STORAGE_KEYS = { USERS:'appUsers', PENDING:'pendingUsers', MENU:'weeklyMenu' };
const DAYS=['segunda','terca','quarta','quinta','sexta'];
const DEFAULT_MENU={
  segunda:{cafe:'Pão e leite', merenda:'Banana', almoco:'Arroz, feijão e frango', cafeTarde:'Bolo e suco', esporte:'Futsal'},
  terca:{cafe:'Biscoito e suco', merenda:'Maçã', almoco:'Macarrão e frango', cafeTarde:'Pão doce', esporte:'Basquete'},
  quarta:{cafe:'Cuscuz com ovo', merenda:'Melancia', almoco:'Arroz, peixe', cafeTarde:'Bolacha', esporte:'Queimada'},
  quinta:{cafe:'Tapioca', merenda:'Pera', almoco:'Arroz, feijão e peixe', cafeTarde:'Bolo de cenoura', esporte:'Vôlei'},
  sexta:{cafe:'Pão com queijo', merenda:'Laranja', almoco:'Feijoada', cafeTarde:'Biscoito', esporte:'Futebol'}
};

// Inicializa localStorage
if(!localStorage.getItem(STORAGE_KEYS.USERS)) localStorage.setItem(STORAGE_KEYS.USERS,JSON.stringify({}));
if(!localStorage.getItem(STORAGE_KEYS.PENDING)) localStorage.setItem(STORAGE_KEYS.PENDING,JSON.stringify([]));
if(!localStorage.getItem(STORAGE_KEYS.MENU)) localStorage.setItem(STORAGE_KEYS.MENU,JSON.stringify(DEFAULT_MENU));

// ===================== PÁGINAS =====================
function showPage(pageId){
  document.querySelectorAll('.container').forEach(c=>c.classList.add('hidden'));
  document.getElementById(pageId).classList.remove('hidden');
}
function logout(){ showPage('chooseLoginPage'); }

// ===================== LOGIN / CADASTRO =====================
function loginUser(){
  const id=document.getElementById('userId').value.trim();
  const senha=document.getElementById('userSenha').value.trim();
  const users=JSON.parse(localStorage.getItem(STORAGE_KEYS.USERS));
  if(users[id]){
    if(users[id].approved){
      showPage('userPage'); loadUserData();
    } else { alert('⏳ Usuário ainda não aprovado pelo administrador.'); }
  } else { alert('❌ ID ou senha incorretos.'); }
}

function loginAdmin(){
  const id=document.getElementById('adminId').value.trim();
  const senha=document.getElementById('adminSenha').value.trim();
  if(id==='admin' && senha==='1234'){
    showPage('adminPage'); loadPendingUsers(); loadAdminData();
  } else { alert('❌ Administrador inválido!'); }
}

function createUser(){
  const id=document.getElementById('newId').value.trim();
  const senha=document.getElementById('newSenha').value.trim();
  if(!id||!senha){ alert('Preencha todos os campos!'); return; }
  const pending=JSON.parse(localStorage.getItem(STORAGE_KEYS.PENDING));
  pending.push({id:id,senha:senha});
  localStorage.setItem(STORAGE_KEYS.PENDING,JSON.stringify(pending));
  alert('✅ Solicitação enviada! Aguarde aprovação do administrador.');
  showPage('chooseLoginPage');
}

// ===================== ADMIN =====================
function loadPendingUsers(){
  const pending=JSON.parse(localStorage.getItem(STORAGE_KEYS.PENDING));
  const list=document.getElementById('pendingList');
  list.innerHTML='';
  pending.forEach((u,i)=>{
    const li=document.createElement('li');
    li.innerHTML=`${u.id} <button onclick="approveUser(${i})">Aprovar</button> <button onclick="rejectUser(${i})">Rejeitar</button>`;
    list.appendChild(li);
  });
}

function approveUser(index){
  const pending=JSON.parse(localStorage.getItem(STORAGE_KEYS.PENDING));
  const users=JSON.parse(localStorage.getItem(STORAGE_KEYS.USERS));
  const user=pending[index];
  users[user.id]={senha:user.senha,approved:true};
  localStorage.setItem(STORAGE_KEYS.USERS,JSON.stringify(users));
  pending.splice(index,1);
  localStorage.setItem(STORAGE_KEYS.PENDING,JSON.stringify(pending));
  loadPendingUsers();
  alert(`${user.id} aprovado!`);
}

function rejectUser(index){
  let pending=JSON.parse(localStorage.getItem(STORAGE_KEYS.PENDING));
  pending.splice(index,1);
  localStorage.setItem(STORAGE_KEYS.PENDING,JSON.stringify(pending));
  loadPendingUsers();
  alert('Usuário rejeitado!');
}

// ===================== CARDÁPIO =====================
function loadUserData(){
  const day=document.getElementById('userDaySelect').value;
  const menu=JSON.parse(localStorage.getItem(STORAGE_KEYS.MENU))[day];
  document.getElementById('dailySport').innerText=menu.esporte;
  document.getElementById('cafeManha').innerText=menu.cafe;
  document.getElementById('merendaManha').innerText=menu.merenda;
  document.getElementById('almoco').innerText=menu.almoco;
  document.getElementById('cafeTarde').innerText=menu.cafeTarde;
}

function loadAdminData(){
  const day=document.getElementById('adminDaySelect').value;
  const menu=JSON.parse(localStorage.getItem(STORAGE_KEYS.MENU))[day];
  document.getElementById('updateSport').value=menu.esporte;
  document.getElementById('updateCafe').value=menu.cafe;
  document.getElementById('updateMerenda').value=menu.merenda;
  document.getElementById('updateAlmoco').value=menu.almoco;
  document.getElementById('updateCafeTarde').value=menu.cafeTarde;
}

function saveMenuAdmin(){
  const day=document.getElementById('adminDaySelect').value;
  const menu=JSON.parse(localStorage.getItem(STORAGE_KEYS.MENU));
  menu[day]={cafe:document.getElementById('updateCafe').value,
             merenda:document.getElementById('updateMerenda').value,
             almoco:document.getElementById('updateAlmoco').value,
             cafeTarde:document.getElementById('updateCafeTarde').value,
             esporte:document.getElementById('updateSport').value};
  localStorage.setItem(STORAGE_KEYS.MENU,JSON.stringify(menu));
  alert('✅ Cardápio atualizado!');
}

// ===================== RELÓGIO =====================
function updateClock(){
  const now=new Date();
  let h=now.getHours(), m=now.getMinutes(), s=now.getSeconds();
  h=h<10?'0'+h:h; m=m<10?'0'+m:m; s=s<10?'0'+s:s;
  document.getElementById('clock').innerText=`${h}:${m}:${s}`;
  const dayNames=['domingo','segunda','terca','quarta','quinta','sexta','sabado'];
  const today=dayNames[now.getDay()];
  document.getElementById('currentDay').innerText=today.charAt(0).toUpperCase()+today.slice(1);
  if(now.getDay()>=1 && now.getDay()<=5){
    document.getElementById('userDaySelect').value=today;
    loadUserData();
  }
}
setInterval(updateClock,1000);
updateClock();
