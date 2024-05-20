from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import *
import sys, math
from pygame import mixer


class PomodoroApp(QWidget):
    def _init_(self):
        super()._init_()

        self.init_ui()
        self.atur_timer()

    def init_ui(self):
        self.alert = QMessageBox()
        self.alert.setWindowTitle('Pemberitahuan!')

        self.flag = False
        self.jumlah_detik = 0

        self.pemanggil_fungsi = QTimer()
        self.pemanggil_fungsi.timeout.connect(self.hitung_mundur)
        self.pemanggil_fungsi.start(1000)

        self.judul = QLabel("Time to Study")
        self.judul.setFont(QFont('Poppins', 30))
        self.judul.setFixedHeight(70)
        self.judul.setAlignment(Qt.AlignCenter)

        # Input fields
        self.ket_waktu_belajar = QLabel('Waktu Belajar')
        self.ket_waktu_belajar.setFixedSize(200, 25)
        self.ket_waktu_belajar.setFont(QFont('Poppins', 9))
        self.ket_waktu_belajar.setAlignment(Qt.AlignCenter)

        self.input_menit_belajar = QLineEdit()
        self.input_menit_belajar.setPlaceholderText('Menit')
        self.input_menit_belajar.setMaxLength(2)
        self.input_menit_belajar.setValidator(QIntValidator())
        self.input_menit_belajar.setFixedSize(95, 30)
        self.input_menit_belajar.setFont(QFont('Poppins', 9))
        self.input_menit_belajar.setAlignment(Qt.AlignCenter)
        self.input_menit_belajar.setStyleSheet('background : white; border-radius : 5; border : 1 solid black')

        self.input_detik_belajar = QLineEdit()
        self.input_detik_belajar.setMaxLength(2)
        self.input_detik_belajar.setValidator(QIntValidator())
        self.input_detik_belajar.setPlaceholderText('Detik')
        self.input_detik_belajar.setFixedSize(95, 30)
        self.input_detik_belajar.setFont(QFont('Poppins', 9))
        self.input_detik_belajar.setAlignment(Qt.AlignCenter)
        self.input_detik_belajar.setStyleSheet('background : white; border-radius : 5; border : 1 solid black')

        self.ket_waktu_jeda = QLabel('Waktu Jeda')
        self.ket_waktu_jeda.setFixedSize(200, 25)
        self.ket_waktu_jeda.setFont(QFont('Poppins', 9))
        self.ket_waktu_jeda.setAlignment(Qt.AlignCenter)

        self.input_menit_jeda = QLineEdit()
        self.input_menit_jeda.setMaxLength(2)
        self.input_menit_jeda.setValidator(QIntValidator())
        self.input_menit_jeda.setPlaceholderText('Menit')
        self.input_menit_jeda.setFixedSize(95, 30)
        self.input_menit_jeda.setFont(QFont('Poppins', 9))
        self.input_menit_jeda.setAlignment(Qt.AlignCenter)
        self.input_menit_jeda.setStyleSheet('background : white; border-radius : 5; border : 1 solid black')

        self.input_detik_jeda = QLineEdit()
        self.input_detik_jeda.setMaxLength(2)
        self.input_detik_jeda.setValidator(QIntValidator())
        self.input_detik_jeda.setPlaceholderText('Detik')
        self.input_detik_jeda.setFixedSize(95, 30)
        self.input_detik_jeda.setFont(QFont('Poppins', 9))
        self.input_detik_jeda.setAlignment(Qt.AlignCenter)
        self.input_detik_jeda.setStyleSheet('background : white; border-radius : 5; border : 1 solid black')

        self.ket_pomodoro = QLabel('Pengulangan sebelum istirahat panjang')
        self.ket_pomodoro.setFixedSize(200, 45)
        self.ket_pomodoro.setFont(QFont('Poppins', 9))
        self.ket_pomodoro.setAlignment(Qt.AlignCenter)

        self.input_pomodoro = QLineEdit()
        self.input_pomodoro.setValidator(QIntValidator())
        self.input_pomodoro.setMaxLength(1)
        self.input_pomodoro.setPlaceholderText('Pengulangan')
        self.input_pomodoro.setFixedSize(200, 30)
        self.input_pomodoro.setFont(QFont('Poppins', 9))
        self.input_pomodoro.setAlignment(Qt.AlignCenter)
        self.input_pomodoro.setStyleSheet('background : white; border-radius : 5; border : 1 solid black')

        self.tombol_hapus = QPushButton("Hapus")
        self.tombol_hapus.setFixedSize(95, 30)
        self.tombol_hapus.clicked.connect(self.atur_timer)
        self.tombol_hapus.setCursor(QCursor(Qt.PointingHandCursor))
        self.tombol_hapus.setStyleSheet('background : red; border-radius : 5; color : white')

        self.tombol_set = QPushButton("Tetapkan")
        self.tombol_set.setFixedSize(95, 30)
        self.tombol_set.clicked.connect(self.tetapkan_timer)
        self.tombol_set.setCursor(QCursor(Qt.PointingHandCursor))
        self.tombol_set.setStyleSheet('background : green; border-radius : 5; color : white')

        # Timer display
        self.ket_mode = QLabel('')
        self.ket_mode.setFixedHeight(30)
        self.ket_mode.setFont(QFont('Poppins', 14))
        self.ket_mode.setAlignment(Qt.AlignCenter)

        self.tampilan_timer = QLabel('')
        self.tampilan_timer.setFixedSize(450, 150)
        self.tampilan_timer.setFont(QFont('Poppins', 65))
        self.tampilan_timer.setAlignment(Qt.AlignCenter)

        self.tombol_reset = QPushButton("Reset")
        self.tombol_reset.setFixedSize(100, 30)
        self.tombol_reset.clicked.connect(self.atur_timer)
        self.tombol_reset.setCursor(QCursor(Qt.PointingHandCursor))
        self.tombol_reset.setStyleSheet('background : red; border-radius : 5; color : white')

        self.tombol_mulai = QPushButton("Mulai")
        self.tombol_mulai.setFixedSize(100, 30)
        self.tombol_mulai.clicked.connect(self.mulai)
        self.tombol_mulai.setCursor(QCursor(Qt.PointingHandCursor))
        self.tombol_mulai.setStyleSheet('background : green; border-radius : 5; color : white')

        self.tombol_pause = QPushButton("Pause")
        self.tombol_pause.setFixedSize(100, 30)
        self.tombol_pause.clicked.connect(self.pause)
        self.tombol_pause.setCursor(QCursor(Qt.PointingHandCursor))
        self.tombol_pause.setStyleSheet('background : white; border-radius : 5; color : black')

        self.tombol_skip = QPushButton('Skip')
        self.tombol_skip.setFixedSize(325, 30)
        self.tombol_skip.clicked.connect(self.skip)
        self.tombol_skip.setCursor(QCursor(Qt.PointingHandCursor))
        self.tombol_skip.setStyleSheet('background : skyblue; border-radius : 5; color : black')
        self.tombol_skip.hide()

        # Menu Panduan button
        self.menu_panduan_button = QPushButton("Menu Panduan")
        self.menu_panduan_button.setFixedSize(200, 30)
        self.menu_panduan_button.clicked.connect(self.show_menu_panduan)
        self.menu_panduan_button.setCursor(QCursor(Qt.PointingHandCursor))
        self.menu_panduan_button.setStyleSheet('background : blue; border-radius : 5; color : white')

        # Layouts
        form_layout = QGridLayout()
        form_layout.addWidget(self.ket_waktu_belajar, 0, 0, 1, 2)
        form_layout.addWidget(self.input_menit_belajar, 1, 0)
        form_layout.addWidget(self.input_detik_belajar, 1, 1)
        form_layout.addWidget(self.ket_waktu_jeda, 2, 0, 1, 2)
        form_layout.addWidget(self.input_menit_jeda, 3, 0)
        form_layout.addWidget(self.input_detik_jeda, 3, 1)
        form_layout.addWidget(self.ket_pomodoro, 4, 0, 2, 2)
        form_layout.addWidget(self.input_pomodoro, 6, 0, 1, 2)
        form_layout.addWidget(self.tombol_hapus, 7, 0)
        form_layout.addWidget(self.tombol_set, 7, 1)

        button_layout = QGridLayout()
        button_layout.addWidget(self.tombol_reset, 0, 0)
        button_layout.addWidget(self.tombol_mulai, 0, 1)
        button_layout.addWidget(self.tombol_pause, 1, 0)
        button_layout.addWidget(self.tombol_skip, 1, 1)

        main_layout = QVBoxLayout()
        main_layout.addWidget(self.judul)
        main_layout.addLayout(form_layout)
        main_layout.addWidget(self.ket_mode)
        main_layout.addWidget(self.tampilan_timer)
        main_layout.addLayout(button_layout)
        main_layout.addWidget(self.menu_panduan_button)

        self.setLayout(main_layout)
        self.setWindowTitle('Pomodoro Timer')
        self.setFixedSize(500, 700)
        self.show()

    def atur_timer(self):
        self.flag = False
        self.ket_mode.setText('')
        self.tampilan_timer.setText('00:00')
        self.pengulangan = 1
        self.pengulangan_pomodoro = 0

    def tetapkan_timer(self):
        try:
            menit_belajar = int(self.input_menit_belajar.text()) if self.input_menit_belajar.text() else 0
            detik_belajar = int(self.input_detik_belajar.text()) if self.input_detik_belajar.text() else 0
            menit_jeda = int(self.input_menit_jeda.text()) if self.input_menit_jeda.text() else 0
            detik_jeda = int(self.input_detik_jeda.text()) if self.input_detik_jeda.text() else 0

            self.batas_belajar = menit_belajar * 60 + detik_belajar
            self.batas_jeda = menit_jeda * 60 + detik_jeda
            self.batas_pengulangan = int(self.input_pomodoro.text()) if self.input_pomodoro.text() else 0
            self.pengulangan = 1
            self.pengulangan_pomodoro = 0
            self.tampilan_timer.setText(f'{menit_belajar:02}:{detik_belajar:02}')
            self.alert.setText("Pengaturan waktu berhasil diubah!")
            self.alert.exec_()
        except:
            self.alert.setText('Masukkan angka dengan benar!')
            self.alert.exec_()

    def mulai(self):
        self.flag = True
        self.jumlah_detik = self.batas_belajar
        self.ket_mode.setText(f'Belajar ke-{self.pengulangan}')
        self.tombol_skip.show()

    def hitung_mundur(self):
        if self.flag:
            if self.jumlah_detik > 0:
                menit = math.floor(self.jumlah_detik / 60)
                detik = self.jumlah_detik % 60
                self.tampilan_timer.setText(f'{menit:02}:{detik:02}')
                self.jumlah_detik -= 1
            else:
                mixer.init()
                mixer.music.load('twice.mp3')
                mixer.music.play()
                self.jumlah_detik = self.batas_jeda if self.pengulangan % 2 else self.batas_belajar
                self.pengulangan += 1
                if self.pengulangan % 2 == 0:
                    self.ket_mode.setText(f'Jeda ke-{self.pengulangan_pomodoro}')
                else:
                    self.ket_mode.setText(f'Belajar ke-{self.pengulangan_pomodoro + 1}')
                    self.pengulangan_pomodoro += 1
                if self.pengulangan_pomodoro > self.batas_pengulangan:
                    self.pengulangan_pomodoro = 0
                    self.ket_mode.setText('Istirahat Panjang')
                    self.flag = False
                    self.tombol_skip.hide()

    def pause(self):
        self.flag = False

    def skip(self):
        self.flag = False
        self.jumlah_detik = 0
        self.ket_mode.setText('')

    def show_menu_panduan(self):
        panduan_message = QMessageBox()
        panduan_message.setWindowTitle("Panduan Penggunaan Aplikasi")
        panduan_message.setText(
            "1. Masukkan waktu belajar dan jeda dalam menit dan detik.\n"
            "2. Masukkan jumlah pengulangan Pomodoro.\n"
            "3. Klik 'Tetapkan' untuk menyimpan pengaturan.\n"
            "4. Klik 'Mulai' untuk memulai timer.\n"
            "5. Klik 'Pause' untuk menjeda timer.\n"
            "6. Klik 'Reset' untuk mengatur ulang timer.\n"
            "7. Klik 'Skip' untuk melewati sesi saat ini.\n"
            "8. Timer akan berulang sesuai pengaturan yang ditetapkan."
        )
        panduan_message.setStandardButtons(QMessageBox.Ok)
        panduan_message.exec_()

if _name_ == '_main_':
    app = QApplication(sys.argv)
    pomodoro_app = PomodoroApp()
    sys.exit(app.exec_())
 

