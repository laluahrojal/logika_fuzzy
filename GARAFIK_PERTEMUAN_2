import numpy as np
import matplotlib.pyplot as plt

# ==========================================================
# FUNGSI KEANGGOTAAN — SESUAI TUGAS + TAMPILAN CONTOH
# ==========================================================
# Bayi    : a=0, b=0, c=2.5, d=5
# Anak-anak: a=-, b=8, c=-, d=12
# Remaja  : a=10, b=12, c=18, d=20
# Pemuda  : a=15, b=18, c=22, d=25
# Dewasa  : a=18, b=20, c=60, d=65
# Lansia  : a=60, b=65, c=75, d=80

x = np.linspace(0, 150, 1000)

mu_bayi    = np.zeros_like(x)
mu_anak    = np.zeros_like(x)
mu_remaja  = np.zeros_like(x)
mu_pemuda  = np.zeros_like(x)
mu_dewasa  = np.zeros_like(x)
mu_lansia  = np.zeros_like(x)

# ---------------------- 1. BAYI ----------------------
mu_bayi[x <= 2.5] = 1
kondisi = (x > 2.5) & (x < 5)
mu_bayi[kondisi] = (-x[kondisi] + 5) / 2.5

# ---------------------- 2. ANAK-ANAK ----------------------
mu_anak[x <= 8] = 1
kondisi = (x > 8) & (x < 12)
mu_anak[kondisi] = (-x[kondisi] + 12) / 4

# ---------------------- 3. REMAJA ----------------------
kondisi = (x > 10) & (x < 12)
mu_remaja[kondisi] = (x[kondisi] - 10) / 2
kondisi = (x >= 12) & (x <= 18)
mu_remaja[kondisi] = 1
kondisi = (x > 18) & (x < 20)
mu_remaja[kondisi] = (-x[kondisi] + 20) / 2

# ---------------------- 4. PEMUDA ----------------------
kondisi = (x > 15) & (x < 18)
mu_pemuda[kondisi] = (x[kondisi] - 15) / 3
kondisi = (x >= 18) & (x <= 22)
mu_pemuda[kondisi] = 1
kondisi = (x > 22) & (x < 25)
mu_pemuda[kondisi] = (-x[kondisi] + 25) / 3

# ---------------------- 5. DEWASA ----------------------
kondisi = (x > 18) & (x < 20)
mu_dewasa[kondisi] = (x[kondisi] - 18) / 2
kondisi = (x >= 20) & (x <= 60)
mu_dewasa[kondisi] = 1
kondisi = (x > 60) & (x < 65)
mu_dewasa[kondisi] = (-x[kondisi] + 65) / 5

# ---------------------- 6. LANSIA ----------------------
kondisi = (x > 60) & (x < 65)
mu_lansia[kondisi] = (x[kondisi] - 60) / 5
kondisi = (x >= 65)
mu_lansia[kondisi] = 1

# ==========================================================
# GRAFIK — TAMPILAN SAMA CONTOH
# ==========================================================
plt.figure(figsize=(12, 6))

plt.plot(x, mu_bayi,   label='Bayi / Balita (0-5 thn)',    linewidth=2)
plt.plot(x, mu_anak,   label='Anak-anak (6-11 thn)',       linewidth=2)
plt.plot(x, mu_remaja, label='Remaja (10-19 thn)',         linewidth=2)
plt.plot(x, mu_pemuda, label='Pemuda (15-24 thn)',         linewidth=2)
plt.plot(x, mu_dewasa, label='Dewasa (20-65 thn)',         linewidth=2)
plt.plot(x, mu_lansia, label='Lansia (>=65 thn)',          linewidth=2)

plt.title("Fungsi Keanggotaan Trapesium (Trapezoidal MF) - Variabel Usia", fontsize=14, pad=15)
plt.xlabel("Usia (Tahun)", fontsize=11)
plt.ylabel("Derajat Keanggotaan mu(x)", fontsize=11)
plt.xlim(0, 150)
plt.ylim(0, 1.05)
plt.xticks(np.arange(0, 151, 10))
plt.yticks(np.arange(0, 1.1, 0.2))
plt.legend(loc='center right', fontsize=10)
plt.grid(True, alpha=0.4, linestyle='--')

plt.tight_layout()
plt.show()
