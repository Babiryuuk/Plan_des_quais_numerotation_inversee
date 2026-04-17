<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Plan des quais</title>
</head>
<body>
  <h1>Bienvenue</h1>
  <p>Voici mon site en ligne.</p>

  <a href="https://www.google.com" target="_blank">
    Lien de test
  </a>
  
  <script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
import {
  getFirestore,
  doc,
  getDoc,
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

const ref = doc(db, "cases", "case1");

// clic = changement partagé
window.toggleCase = async () => {
  const snap = await getDoc(ref);
  const active = snap.exists() && snap.data().active;
  await setDoc(ref, { active: !active });
};

// mise à jour pour tous
onSnapshot(ref, snap => {
  const el = document.getElementById("case1");
  if (!el) return;
  el.style.background =
    snap.exists() && snap.data().active ? "green" : "white";
});
</script>
</body>
</html>
``
