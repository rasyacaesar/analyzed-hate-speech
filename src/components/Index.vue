<script setup>
import { GoogleGenAI } from "@google/genai";
import { ref } from "vue";
import Spinner from "./Spinner.vue";

const apiKey = import.meta.env.VITE_GEMINI_API_KEY;
const ai = new GoogleGenAI({apiKey});
const result = ref('');
const isLoading = ref(false);

async function analyze() {
  try {
    isLoading.value = true;

    const input = document.getElementById("inputText").value.toLowerCase();
    const prompt = `
      Anda adalah sistem deteksi ujaran kebencian. 
      Analisis teks yang diberikan dan tentukan apakah mengandung ujaran kebencian atau tidak. 
      Respon HANYA dengan klasifikasi sederhana.
      Kriteria Ujaran Kebencian
      Teks dianggap Ujaran Kebencian jika mengandung:

      Hinaan atau serangan terhadap kelompok berdasarkan ras, agama, gender, etnis, orientasi seksual, disabilitas
      Bahasa yang merendahkan atau mendehumanisasi kelompok tertentu
      Stereotip negatif yang menyakitkan terhadap kelompok minoritas
      Ajakan diskriminasi atau pengucilan
      Ancaman atau hasutan kekerasan
      Ujaran yang dapat memicu kebencian atau permusuhan antar kelompok

      Teks dianggap Bukan Ujaran Kebencian jika:

      Kritik konstruktif atau pendapat yang tidak menyerang identitas kelompok
      Konten netral, positif, atau informatif
      Diskusi faktual tanpa maksud merendahkan
      Humor yang tidak menargetkan kelompok rentan

      Teksnya adalah ${input}
    `

    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash",
      contents: prompt,
    });

    result.value = response.text;
  } catch (e) {
    return;
  } finally {
    isLoading.value = false;
  }
}
</script>

<template>
  <header>
    <h1>Cyberbullying & Hate Speech</h1>
    <p>Edukasi Cyberbullying dan Hate Speech</p>
  </header>

  <div class="container">
    <h2>📌 Pengertian Cyberbullying</h2>
    <p>Cyberbullying adalah tindakan agresif yang dilakukan melalui media digital seperti media sosial, chat, atau forum online, dengan tujuan menyakiti, mempermalukan, atau mengintimidasi orang lain.</p>

    <h2>⚠️ Dampak & Akibat Cyberbullying</h2>
    <div class="highlight">
      <ul>
        <li>🧠 Gangguan mental seperti stres, depresi, dan kecemasan</li>
        <li>🚫 Penurunan rasa percaya diri dan isolasi sosial</li>
        <li>📉 Prestasi akademik menurun</li>
        <li>💔 Dalam kasus ekstrem, bisa menyebabkan tindakan bunuh diri</li>
      </ul>
    </div>

    <h2>📌 Pengertian Hate Speech</h2>
    <p>Hate speech adalah ujaran kebencian yang menyerang individu atau kelompok berdasarkan ras, agama, gender, orientasi seksual, atau identitas lainnya. Biasanya disampaikan melalui kata-kata kasar, hinaan, atau provokasi.</p>

    <h2>⚠️ Dampak & Akibat Hate Speech</h2>
    <div class="highlight">
      <ul>
        <li>🔥 Memicu konflik sosial dan kekerasan</li>
        <li>🧠 Menyebabkan trauma psikologis pada korban</li>
        <li>🌐 Merusak iklim komunikasi digital yang sehat</li>
        <li>⚖️ Bisa berujung pada pelanggaran hukum dan pidana</li>
      </ul>
    </div>

    <h2>🧪 Simulasi Deteksi Komentar</h2>
    <p>Masukkan komentar online untuk dianalisis oleh sistem:</p>
    <textarea id="inputText" placeholder="Contoh: Kamu bodoh dan tidak pantas di sini..."></textarea>
    <button @click="analyze()">
      <Spinner v-if="isLoading" size="small"/>
      Analisis Komentar
    </button>
    <div class="result" id="resultText">Teks tersebut termasuk ke dalam {{ result }}</div>
  </div>
</template>

<style>
    body {
      font-family: 'Inter', sans-serif;
      background: linear-gradient(to right, #e3f2fd, #fce4ec);
      margin: 0;
      padding: 0;
    }
    header {
      background-color: #1a237e;
      color: white;
      padding: 30px;
      text-align: center;
    }
    header p {
      color: white;
    }
    .container {
      margin: 30px auto;
      background: white;
      padding: 40px;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(0,0,0,0.1);
    }
    h2 {
      color: #1a237e;
      margin-top: 40px;
    }
    p {
      font-size: 16px;
      line-height: 1.6;
      color: #333;
    }
    textarea {
      width: 100%;
      height: 100px;
      padding: 12px;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #ccc;
      margin-top: 20px;
    }
    button {
      background-color: #3949ab;
      color: white;
      padding: 12px 24px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-weight: bold;
      margin-top: 10px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 0.5rem;
    }
    button:hover {
      background-color: #283593;
    }
    .result {
      margin-top: 20px;
      font-weight: bold;
      font-size: 18px;
    }
    .highlight {
      background: #f3f3f3;
      padding: 15px;
      border-left: 5px solid #3949ab;
      border-radius: 8px;
      margin-top: 20px;
    }
</style>