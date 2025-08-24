@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

body { 
    font-family:'Roboto', sans-serif; 
    margin:0; padding:0; 
    background:#e0f7fa; 
}

header { 
    background: linear-gradient(135deg,#00796b,#004d40); 
    color:#fff; text-align:center; 
    padding:25px 20px; font-size:28px; 
    font-weight:700; border-bottom-left-radius:20px; 
    border-bottom-right-radius:20px; 
    box-shadow:0 4px 8px rgba(0,0,0,0.2);
}

.container { padding:20px; }

.card { 
    background:#fff; margin:15px 0; padding:20px; 
    border-radius:15px; box-shadow:0 4px 8px rgba(0,0,0,0.1); 
    transition:0.3s; 
}

.card:hover { 
    transform:translateY(-3px); 
    box-shadow:0 6px 12px rgba(0,0,0,0.15); 
}

h2 { color:#004d40; margin-top:0; }

input, select { 
    width:100%; padding:12px; margin:8px 0; 
    border-radius:8px; border:1px solid #ccc; font-size:16px; 
}

.btn { 
    background:#00796b; color:white; border:none; 
    padding:14px; margin-top:10px; width:100%; 
    border-radius:12px; font-size:16px; cursor:pointer; transition:0.3s; 
}

.btn:hover { background:#004d40; }

.back { background:#d32f2f; }
.back:hover { background:#b71c1c; }

.hidden { display:none; }
ul { padding-left:20px; }
