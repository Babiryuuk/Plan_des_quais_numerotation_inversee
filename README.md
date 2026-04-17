<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Plan des quais – numérotation inversée</title>

  <style>
    body {
      margin:0;
      font-family:Arial, sans-serif;
      background:#f4f4f4;
    }

    #container {
      position:relative;
      width:1200px;
      margin:auto;
      margin-top:40px;
    }

    .dock {
      position:absolute;
      width:90px;
      height:36px;
      border-radius:8px;
      display:flex;
      align-items:center;
      justify-content:center;
      color:white;
      font-weight:bold;
      cursor:pointer;
      user-select:none;
      box-shadow:0 2px 4px rgba(0,0,0,0.3);
    }

    .red { background:#c0392b; }
    .green { background:#27ae60; }
  </style>
</head>

<body>

<div id="container">
  <!-- IMAGE DU PLAN -->
  <img src="Capture d'écran(18)" alt="Plan des quais">

  <!-- BOUTONS AU-DESSUS DU PLAN -->
  <div class="dock red" data-id="Q01" style="top:420px; left:180px;">01</div>
  <div class="dock red" data-id="Q02" style="top:420px; left:260px;">02</div>
  <div class="dock red" data-id="Q03" style="top:420px; left:340px;">03</div>
</div>


<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import {
    getFirestore,
    doc,
    setDoc,
    onSnapshot
  } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBpKGxGcC6IHvNTu9bpTwi_4jZIAF5olg",
    authDomain: "projet-plan-dynamique-pour-ma.firebaseapp.com",
    projectId: "projet-plan-dynamique-pour-ma",
    storageBucket: "projet-plan-dynamique-pour-ma.appspot.com",
    messagingSenderId: "1008307493478",
    appId: "1:1008307493478:web:e7fcc54f4bba624da26ba7"
  };

  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);

  // 🔁 Synchronisation temps réel des quais
  document.querySelectorAll(".dock").forEach(dock => {
    const id = dock.dataset.id;
    const ref = doc(db, "quais", id);

    // ✅ clic → écriture Firestore
    dock.addEventListener("click", async () => {
      await setDoc(ref, { etat: "green" }, { merge: true });
    });

    // ✅ TEMPS RÉEL → lecture Firestore
    onSnapshot(ref, snap => {
      dock.classList.remove("red", "green");
      if (snap.exists()) {
        dock.classList.add(snap.data().etat);
      } else {
        dock.classList.add("red");
      }
    });
  });
</script>

</body>
</html>
``
