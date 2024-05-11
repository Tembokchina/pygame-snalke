# pygame-snake
import pygame
import time
import random

pygame.init()

# Variabel global
lebar = 800
tinggi = 600
ukuran_blok = 20
kecepatan = 15

warna_putih = (255, 255, 255)
warna_hitam = (0, 0, 0)
warna_merah = (213, 50, 80)
warna_hijau = (0, 255, 0)

layar = pygame.display.set_mode((lebar, tinggi))
pygame.display.set_caption('Snake')

font_standar = pygame.font.SysFont("comicsansms", 35)

def pesan_teks(msg, warna):
    teks = font_standar.render(msg, True, warna)
    return teks, teks.get_rect()

def tampilkan_skor(skor):
    teks_skor = font_standar.render("Skor: " + str(skor), True, warna_putih)
    layar.blit(teks_skor, [0, 0])

def ular(ukuran_blok, daftar_ular):
    for x in daftar_ular:
        pygame.draw.rect(layar, warna_hijau, [x[0], x[1], ukuran_blok, ukuran_blok])

def batas_sangkar():
    # Batas atas dan bawah
    for x in range(4 * ukuran_blok, lebar - 4 * ukuran_blok, ukuran_blok):
        pygame.draw.rect(layar, warna_putih, [x, tinggi - 26 * ukuran_blok, ukuran_blok, ukuran_blok])
        pygame.draw.rect(layar, warna_putih, [x, tinggi - 5 * ukuran_blok, ukuran_blok, ukuran_blok])
    # Batas kiri dan kanan
    for y in range(4 * ukuran_blok, tinggi - 4 * ukuran_blok, ukuran_blok):
        pygame.draw.rect(layar, warna_putih, [lebar - 37 * ukuran_blok, y, ukuran_blok, ukuran_blok])
        pygame.draw.rect(layar, warna_putih, [lebar - 5 * ukuran_blok, y, ukuran_blok, ukuran_blok])

def game_over(daftar_ular):
    kepala = daftar_ular[-1]
    x, y = kepala[0], kepala[1]
    if x < 4 * ukuran_blok or x >= lebar - 4 * ukuran_blok or y < 4 * ukuran_blok or y >= tinggi - 4 * ukuran_blok:
        return True
    return False

def game_loop():
    while True:
        game_end = False

        # Posisi awal ular
        x_ular = lebar / 2
        y_ular = tinggi / 2

        # Perubahan posisi awal ular
        delta_x = 0
        delta_y = 0

        # Posisi makanan awal
        makanan_x = round(random.randrange(4 * ukuran_blok, lebar - 4 * ukuran_blok) / ukuran_blok) * ukuran_blok
        makanan_y = round(random.randrange(4 * ukuran_blok, tinggi - 4 * ukuran_blok) / ukuran_blok) * ukuran_blok

        # Daftar untuk menampung bagian-bagian dari ular
        daftar_ular = []
        panjang_ular = 1

        # Skor
        skor = 0

        while not game_end:

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    quit()
                elif event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_LEFT:
                        delta_x = -ukuran_blok
                        delta_y = 0
                    elif event.key == pygame.K_RIGHT:
                        delta_x = ukuran_blok
                        delta_y = 0
                    elif event.key == pygame.K_UP:
                        delta_y = -ukuran_blok
                        delta_x = 0
                    elif event.key == pygame.K_DOWN:
                        delta_y = ukuran_blok
                        delta_x = 0
                    elif event.key == pygame.K_c:
                        # Mengatur ulang permainan
                        return

            x_ular += delta_x
            y_ular += delta_y
            layar.fill(warna_hitam)
            batas_sangkar()
            pygame.draw.rect(layar, warna_merah, [makanan_x, makanan_y, ukuran_blok, ukuran_blok])
            daftar_ular.append([x_ular, y_ular])
            if len(daftar_ular) > panjang_ular:
                del daftar_ular[0]

            if game_over(daftar_ular):
                game_end = True

            for blok in daftar_ular[:-1]:
                if blok == daftar_ular[-1]:
                    game_end = True

            if x_ular == makanan_x and y_ular == makanan_y:
                panjang_ular += 1
                skor += 10
                makanan_x = round(random.randrange(3 * ukuran_blok, lebar - 3 * ukuran_blok) / ukuran_blok) * ukuran_blok
                makanan_y = round(random.randrange(3 * ukuran_blok, tinggi - 3 * ukuran_blok) / ukuran_blok) * ukuran_blok

            ular(ukuran_blok, daftar_ular)
            tampilkan_skor(skor)
            pygame.display.update()
            time.sleep(0.1)

        layar.fill(warna_hitam)
        pesan1, pesan1_rect = pesan_teks("Permainan berakhir!, warna_putih)
        layar.blit(pesan1, (lebar / 6, tinggi / 3))
        pesan2, pesan2_rect = pesan_teks("Skor: " + str(skor), warna_putih)
        layar.blit(pesan2, (lebar / 2.5, tinggi / 2))
        pygame.display.update()

game_loop()
