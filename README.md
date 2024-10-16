import pygame
import time
import random

# Pygame'i başlat
pygame.init()

# Renk tanımlamaları
beyaz = (255, 255, 255)
siyah = (0, 0, 0)
kirmizi = (213, 50, 80)
yesil = (0, 255, 0)
mavi = (50, 153, 213)

# Oyun ayarları
genislik = 600
yukseklik = 400
ekran = pygame.display.set_mode((genislik, yukseklik))
pygame.display.set_caption('Yılan Oyunu')

# Yılan ayarları
yilan_boyutu = 10
yilan_hizi = 15

# Font ayarları
font_stil = pygame.font.SysFont("bahnschrift", 25)
skor_font = pygame.font.SysFont("comicsansms", 35)

def skor_goster(skor):
    deger = skor_font.render("Skor: " + str(skor), True, siyah)
    ekran.blit(deger, [0, 0])

def yilan(cisimler):
    for x in cisimler:
        pygame.draw.rect(ekran, yesil, [x[0], x[1], yilan_boyutu, yilan_boyutu])

def mesaj(text):
    mesaj_ekran = font_stil.render(text, True, kirmizi)
    ekran.blit(mesaj_ekran, [genislik / 6, yukseklik / 3])

def oyun():
    oyun_bitti = False
    oyun_kapandi = False

    x = genislik / 2
    y = yukseklik / 2

    x_hizi = 0
    y_hizi = 0

    yilan_listesi = []
    uzunluk = 1

    yemek_x = round(random.randrange(0, genislik - yilan_boyutu) / 10.0) * 10.0
    yemek_y = round(random.randrange(0, yukseklik - yilan_boyutu) / 10.0) * 10.0

    while not oyun_bitti:

        while oyun_kapandi:
            ekran.fill(beyaz)
            mesaj("Kaybettin! Q-Çık, C-Yeniden Oyna")
            skor_goster(uzunluk - 1)
            pygame.display.update()

            for olay in pygame.event.get():
                if olay.type == pygame.KEYDOWN:
                    if olay.key == pygame.K_q:
                        oyun_bitti = True
                        oyun_kapandi = False
                    if olay.key == pygame.K_c:
                        oyun()

        for olay in pygame.event.get():
            if olay.type == pygame.QUIT:
                oyun_bitti = True
            if olay.type == pygame.KEYDOWN:
                if olay.key == pygame.K_LEFT:
                    x_hizi = -yilan_boyutu
                    y_hizi = 0
                elif olay.key == pygame.K_RIGHT:
                    x_hizi = yilan_boyutu
                    y_hizi = 0
                elif olay.key == pygame.K_UP:
                    y_hizi = -yilan_boyutu
                    x_hizi = 0
                elif olay.key == pygame.K_DOWN:
                    y_hizi = yilan_boyutu
                    x_hizi = 0

        if x >= genislik or x < 0 or y >= yukseklik or y < 0:
            oyun_kapandi = True

        x += x_hizi
        y += y_hizi
        ekran.fill(beyaz)
        pygame.draw.rect(ekran, kirmizi, [yemek_x, yemek_y, yilan_boyutu, yilan_boyutu])
        yilan_konu = []
        yilan_konu.append(x)
        yilan_konu.append(y)
        yilan_listesi.append(yilan_konu)
        if len(yilan_listesi) > uzunluk:
            del yilan_listesi[0]

        for x in yilan_listesi[:-1]:
            if x == yilan_konu:
                oyun_kapandi = True

        yilan(yilan_listesi)
        skor_goster(uzunluk - 1)

        pygame.display.update()

        if x == yemek_x and y == yemek_y:
            yemek_x = round(random.randrange(0, genislik - yilan_boyutu) / 10.0) * 10.0
            yemek_y = round(random.randrange(0, yukseklik - yilan_boyutu) / 10.0) * 10.0
            uzunluk += 1

        pygame.time.Clock().tick(yilan_hizi)

    pygame.quit()
    quit()
