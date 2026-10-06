import numpy as np
import matplotlib.pyplot as plt

# ASESMEN MODUL 1 - PEMODELAN FUZZY
# Nama : Eka Yuliana Rizky
# NIM  : 240306011

def trimf(x, a, b, c):
    """Fungsi keanggotaan segitiga."""
    x = np.asarray(x, dtype=float)
    y = np.zeros_like(x)

    if b > a:
        idx = (x >= a) & (x <= b)
        y[idx] = (x[idx] - a) / (b - a)

    if c > b:
        idx = (x >= b) & (x <= c)
        y[idx] = np.maximum(y[idx], (c - x[idx]) / (c - b))

    y[x == b] = 1.0
    return np.clip(y, 0, 1)


def trapmf(x, a, b, c, d):
    """Fungsi keanggotaan trapesium, termasuk left/right shoulder."""
    x = np.asarray(x, dtype=float)
    y = np.zeros_like(x)

    # Sisi naik. Jika a=b, fungsi merupakan left shoulder.
    if b > a:
        idx = (x >= a) & (x <= b)
        y[idx] = (x[idx] - a) / (b - a)
    else:
        y[x <= c] = 1.0

    # Bagian datar.
    y[(x >= b) & (x <= c)] = 1.0

    # Sisi turun. Jika c=d, fungsi merupakan right shoulder;
    # bagian datar di atas sudah menangani wilayah b sampai c.
    if d > c:
        idx = (x >= c) & (x <= d)
        y[idx] = np.maximum(y[idx], (d - x[idx]) / (d - c))

    return np.clip(y, 0, 1)


def membership_value(value, kind, params):
    arr = np.array([float(value)])
    if kind == "trimf":
        return float(trimf(arr, *params)[0])
    return float(trapmf(arr, *params)[0])


VARIABEL = {
    "Latensi": {
        "semesta": (0, 300),
        "membership": {
            "Rendah": ("trapmf", (0, 0, 50, 100)),
            "Sedang": ("trimf", (75, 125, 175)),
            "Tinggi": ("trapmf", (150, 200, 300, 300)),
        },
    },
    "Packet Loss": {
        "semesta": (0, 10),
        "membership": {
            "Rendah": ("trapmf", (0, 0, 1, 3)),
            "Sedang": ("trimf", (2, 4, 6)),
            "Tinggi": ("trapmf", (5, 7, 10, 10)),
        },
    },
    "Kualitas Jaringan": {
        "semesta": (0, 100),
        "membership": {
            "Buruk": ("trapmf", (0, 0, 30, 50)),
            "Cukup": ("trimf", (40, 60, 80)),
            "Baik": ("trapmf", (70, 85, 100, 100)),
        },
    },
}


def fuzzifikasi(input_dict):
    latensi = float(input_dict["latensi"])
    packet_loss = float(input_dict["packet_loss"])

    return {
        "latensi": {
            label: membership_value(latensi, kind, params)
            for label, (kind, params)
            in VARIABEL["Latensi"]["membership"].items()
        },
        "packet_loss": {
            label: membership_value(packet_loss, kind, params)
            for label, (kind, params)
            in VARIABEL["Packet Loss"]["membership"].items()
        },
    }


def plot_variabel(nama, spec, filename):
    lo, hi = spec["semesta"]
    x = np.linspace(lo, hi, 1001)

    plt.figure(figsize=(8.5, 5.0))

    for label, (kind, params) in spec["membership"].items():
        y = trimf(x, *params) if kind == "trimf" else trapmf(x, *params)
        # Tampilkan kurva dan parameter titik pembentuk fungsi keanggotaan.
        line, = plt.plot(x, y, linewidth=2, label=label)

        # Tampilkan titik-titik parameter yang benar-benar membentuk kurva.
        # Angka berasal langsung dari parameter membership function.
        if kind == "trimf":
            titik = [
                (params[0], 0),
                (params[1], 1),
                (params[2], 0)
            ]
        else:
            a, b, c, d = params
            if a == b:  # left shoulder
                titik = [(a, 1), (c, 1), (d, 0)]
            elif c == d:  # right shoulder
                titik = [(a, 0), (b, 1), (c, 1)]
            else:
                titik = [(a, 0), (b, 1), (c, 1), (d, 0)]

        for px, py in titik:
            plt.scatter(px, py, s=18, zorder=5, label="_nolegend_")
            offset_y = 8 if py == 0 else -18
            plt.annotate(
                f"({px:g}, {py:g})",
                (px, py),
                xytext=(0, offset_y),
                textcoords="offset points",
                ha="center",
                fontsize=8
            )

    unit = "ms" if nama == "Latensi" else "%" if nama == "Packet Loss" else "poin"
    plt.xlabel(f"{nama} ({unit})")
    plt.ylabel("Derajat Keanggotaan")
    plt.title(f"Fungsi Keanggotaan {nama}")
    plt.xlim(lo, hi)
    plt.ylim(-0.02, 1.05)
    plt.grid(True, alpha=0.25)
    # Legend hanya menampilkan kurva, tanpa marker titik parameter.
    # Parameter fungsi keanggotaan dicantumkan pada label masing-masing kurva.
    legend_labels = []
    for label, (kind, params) in spec["membership"].items():
        bentuk = "Segitiga" if kind == "trimf" else "Trapesium"
        parameter = ", ".join(f"{p:g}" for p in params)
        legend_labels.append(f"{label} ({bentuk}): [{parameter}]")

    handles = []
    for line, legend_text in zip(plt.gca().lines, legend_labels):
        line.set_label(legend_text)
        handles.append(line)

    plt.legend(
        handles=handles,
        labels=legend_labels,
        title="Label & parameter",
        fontsize=8,
        title_fontsize=9
    )
    plt.tight_layout()
    plt.savefig(filename, dpi=200, bbox_inches="tight")
    # Jangan memblokir program pada grafik pertama; semua grafik ditampilkan bersama di akhir.
    plt.show(block=False)
    plt.pause(0.1)


data_uji = [
    {"latensi": 20, "packet_loss": 0.5},
    {"latensi": 90, "packet_loss": 2.5},
    {"latensi": 125, "packet_loss": 4.0},
    {"latensi": 170, "packet_loss": 6.5},
    {"latensi": 290, "packet_loss": 9.5},
]

for data in data_uji:
    print("Input:", data)
    print("Hasil:", fuzzifikasi(data))
    print("-" * 60)


# Menampilkan dan menyimpan seluruh grafik fungsi keanggotaan.
plot_variabel(
    "Latensi",
    VARIABEL["Latensi"],
    "grafik_latensi_final.png"
)

plot_variabel(
    "Packet Loss",
    VARIABEL["Packet Loss"],
    "grafik_packet_loss_final.png"
)

plot_variabel(
    "Kualitas Jaringan",
    VARIABEL["Kualitas Jaringan"],
    "grafik_kualitas_jaringan_final.png"
)

# Tampilkan ketiga grafik sekaligus setelah seluruh grafik selesai dibuat.
plt.show()
