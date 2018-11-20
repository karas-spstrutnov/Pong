# Pong
První verze zamrzá po pohybu pálkou.

import pygame
import sys
from pygame.locals import *
from pygame.event import *

#parametry aplikace
rozliseni_okna = (640, 480)
titulek_okna = 'Pong'

barva_pozadi = (255, 255, 255)
barva_palky = (0, 0, 0)

sirka_palky = 15
vyska_palky = 75

pozice_x_palky = 30
pozice_y_palky = rozliseni_okna[1] / 2

pohyb_palkou_nahoru = False
pohyb_palkou_dolu = False

rychlost_posunu_palky = 0.5
#-------------------------------------------------------------
#POMOCNÉ PROGRAMY
#----------------------------------------------------------
def zpracovani_udalosti():
    #podprogram pouziva proměnné definovane vyse
    global pohyb_palkou_dolu, pohyb_palkou_nahoru
    
    for udalost in pygame.event.get():
        
        #nalezení udalosti zavření okna
        if udalost.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        
        #pokud je nalezena udalost stisknout klavesy...
        if udalost.type == pygame.KEYDOWN:
            if udalost.key == pygame.K_DOWN:
                pohyb_palkou_dolu = True
            if udalost.key == pygame.K_UP:
                pohyb_palkou_nahoru = True
                
            if udalost.key == pygame.K_ESCAPE:
                pygame.quit()
                sys.exit()

        if udalost.type == pygame.KEYUP:
            if udalost.type == pygame.K_UP:
                pohyb_palkou_nahoru = False
            if udalost.type == pygame.K_DOWN:
                pohyb_palkou_dolu = False

def vykreslovaci_operace():
    #vyplnění okna barvou(pozadí)
    okno.fill(barva_pozadi)
    
    #vykreslení pálky
    x = pozice_x_palky - sirka_palky / 2
    y = pozice_y_palky - vyska_palky / 2
    pygame.draw.rect(okno, barva_palky, (x, y, sirka_palky, vyska_palky))
    
def pohyb_palky():
    #používá promněnou definovanou výše
    global pozice_y_palky
    
    #pokud byl detekován pohyb palkou dolu
    if pohyb_palkou_dolu:
        #...posune se palka směrem od horniho okraje dolu
        pozice_y_palky += rychlost_posunu_palky

    #pokud byl detekovan pohyb palkou nahoru
    if pohyb_palkou_nahoru:
        pozice_y_palky -= rychlost_posunu_palky

    #vypocti kolizi palky k hornimu okraji
    horni_okraj_okna = 0
    spodni_okraj_okna = rozliseni_okna[1]
    horni_okraj_palky = pozice_y_palky - vyska_palky / 2
    spodni_okraj_palky = pozice_y_palky + vyska_palky / 2
    
    #pokud horní okraj přesahuje pálka
    if horni_okraj_palky < horni_okraj_okna:
        # ...presune se pálka tak, aby se spodnim omkrajem dotýkala okraje okna
        pozice_y_palky = vyska_palky / 2

    if spodni_okraj_palky > spodni_okraj_okna:
        pozice_y_palky = rozliseni_okna[1] - vyska_palky / 2

def pohyb_micku():
    #placeholder pro pozdější zaplnění
    pass
#---------------------------------------------------------------------------------

#INICIALIZACE

#inicializace knihovny pygame
pygame.init()
#nastavení okna
okno = pygame.display.set_mode(rozliseni_okna)
#nastavení titulku_okna
pygame.display.set_caption(titulek_okna)

#---------------------------------------------------------------------------------
#nekonečné vykreslovaní okna
#---------------------------------
while True:
    zpracovani_udalosti()
    pohyb_palky()
    pohyb_micku()
    vykreslovaci_operace()
    
    
    #frame
    pygame.display.update()
    
