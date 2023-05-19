# SC-Tugas-kelompok7-progam-assembler

cls macro
    mov ax,0600h
    xor cx,cx
    mov dx,184fh
    mov bh,7
    int 10h
endm

gotoxy macro x,y
       mov ah,02
       xor bx,bx
       mov dh,y
       mov dl,x
       int 10h
endm
simpanl macro
        local ulang
        mov ax,0b800h
        mov es,ax
        mov cx,4000
        xor bx,bx

ulang    :
        mov al,es:[bx]
        mov layar[bx],al
        inc bx
        loop ulang
endm
balikl macro
        local ulang
        mov cx,4000
        xor bx,bx

ulang    :
        mov al,layar[bx]
        mov es:[bx],al
        inc bx
        loop ulang
endm

sorot     macro x,y
        local ulang

        mov bl,y
        mov al,160
        mul bl
        mov bx,ax

        mov al,x
        mov ah,2
        mul ah
        add bx,ax
        inc bx

        mov cx,22
ulang    :
        mov byte ptr es:[bx],6fh

        add bx,2
        loop ulang
endm

readkey    macro
        mov ah,00
        int 16h
endm

menul    macro string
        mov ah,09
        lea dx,string
        int 21h
endm


delay macro
    push cx
    xor cx,cx
loop1    :
        loop loop1
        pop cx
endm

Geser    macro PosY
        push ax
        push bx
        push cx

        xor cx,cx
        mov al,26
        sub al,PosY
        mov cl,al

loop2    :
        mov al,byte ptr es:[bx]
        mov byte ptr es:[bx-160],al

hilang    :
        mov byte ptr es:[bx],' '

        delay
        sub bx,160
       
        loop loop2

        pop cx
        pop bx
        pop ax
endm

menu2 macro string
mov ah, 09h
lea dx, string
int 21h
endm

tampilk macro nilai
mov ah, 02h
mov dx, nilai
int 21h
endm

ulangtampil macro nilai, batas
local ulangk
mov cx, batas
ulangk:
mov ah, 02h
mov dx, nilai
int 21h
loop ulangk
endm

delayk macro
local ulangk
local ulang2
push cx
mov cx,0ffffh
ulangk:
push cx
mov cx, 0fffh
ulang2: loop ulang2
pop cx
loop ulangk
pop cx
endm

.model small
.code
org 100h

tdata :
        jmp proses
        layar db 4000 dup (?)
        menu db 9,9,'+==================================+',13,10
             db 9,9,'|      >> >> Menu Sorot << <<      |',13,10
             db 9,9,'+==================================+',13,10
             db 9,9,'|  1. Cetak Karakter               |',13,10
             db 9,9,'|  2. Karakter Berwarna            |',13,10
             db 9,9,'|  3. Program Rontok               |',13,10
db 9,9,'|  4. Anggota Kelompok             |',13,10
db 9,9,'|  5. klasemen liga Inggris        |',13,10
             db 9,9,'|  6. klasemen liga Italia         |',13,10
             db 9,9,'|  7. klasemen liga Spanyol        |',13,10
             db 9,9,'|  8. klasemen liga Jerman         |',13,10
db 9,9,'|  9. klasemen liga Belanda        |',13,10
db 9,9,'|  10. kalkulator 1 digit          |',13,10
             db 9,9,'|  11. Keluar                      |',13,10
             db 9,9,'|                                  |',13,10
             db 9,9,'+==================================+$'

        posx db 22
        posy db 12
    panah_atas equ 72
    panah_bawah equ 80
    tenter      equ 0dh

    proses    :
            cls
            gotoxy 0 10
            menul menu
            simpanl

 ulang    :
            balikl

            sorot posx,posy

    masukan :
            readkey
            cmp ah,panah_bawah
            je bawah

            cmp ah,panah_atas
            je ceky

            cmp al,tenter

            jne masukan
            jmp banding
   

    ceky    :
            cmp posy,12
            je maxy
            dec posy
            jmp ulang

   
    maxy    :
            mov posy,22
            jmp ulang

    bawah     :
            cmp posy,22
            je no1y
            inc posy
            jmp ulang

    no1y    :
            mov posy,12
            jmp ulang
banding    :
            cmp posy,12
            je karakter
            cmp posy,13
            je warna
            cmp posy,14
            je rontok
cmp posy,15
            je anggota
cmp posy, 16
je inggris
            cmp posy,17
            je italia
            cmp posy,18
            je spanyol
cmp posy,19
            je jerman
cmp posy, 20
je belanda
cmp posy, 21
je kalkulator
cmp posy, 22
je keluar

    keluar    :
            jmp selesai
anggota :
jmp anggota1
karakter :
jmp karakter1
warna :
jmp warna1
rontok :
jmp rontok1
inggris :
jmp inggris1
italia :
jmp italia1
spanyol :
jmp spanyol1
jerman :
jmp jerman1
belanda :
jmp belanda1
kalkulator :
jmp kalkulator1
anggota1 :
cls
        gotoxy 22, 12

        mov ah, 09h
        lea dx, anggota_nama
        int 21h

        readkey
        jmp proses

anggota_nama db 'Nama Anggota',13,10
db 9,9,'  1. Erik Romansyah       - 19171065054  ',13,10
db 9,9,'  2. Halim Muzayyin Hakim - 20171065054  ',13,10
db 9,9,'  3. Bagas Kurniawan      - 22171065714  ',13,10
db 9,9,'  4. Jordy Dwi Prakoso    - 22171065713  ',13,10
db 9,9,'  Tekan enter untuk kembali ke menu $',13,10

inggris1 :
cls
        gotoxy 22, 12

        mov ah, 09h
        lea dx, inggris1_nama
        int 21h

        readkey
        jmp proses

inggris1_nama db 'klasemen Liga Inggris',13,10
db 9,9,'  1. Manchester City               ',13,10
db 9,9,'  2. Arsenal             ',13,10
db 9,9,'  3. Newcastle United                ',13,10
db 9,9,'  4. Manchester United               ',13,10
db 9,9,'  Tekan enter untuk kembali ke menu $',13,10


italia1 :
cls
        gotoxy 22, 12

        mov ah, 09h
        lea dx, italia1_nama
        int 21h

        readkey
        jmp proses

italia1_nama db 'klasemen Liga Italia',13,10
db 9,9,'  1. Napoli               ',13,10
db 9,9,'  2. Juventus             ',13,10
db 9,9,'  3. Inter Milan                 ',13,10
db 9,9,'  4. Lazio               ',13,10
db 9,9,'  Tekan enter untuk kembali ke menu $',13,10


spanyol1 :
cls
        gotoxy 22, 12

        mov ah, 09h
        lea dx, spanyol1_nama
        int 21h

        readkey
        jmp proses

spanyol1_nama db 'klasemen Liga Spanyol',13,10
db 9,9,'  1. Barcelona               ',13,10
db 9,9,'  2. Real Madrid             ',13,10
db 9,9,'  3. Atletico Madrid                 ',13,10
db 9,9,'  4. Real Sociedad               ',13,10
db 9,9,'  Tekan enter untuk kembali ke menu $',13,10


jerman1 :
cls
        gotoxy 22, 12

        mov ah, 09h
        lea dx, jerman1_nama
        int 21h

        readkey
        jmp proses

jerman1_nama db 'klasemen Liga Jerman',13,10
db 9,9,'  1. Bayern Munchen               ',13,10
db 9,9,'  2. Dortmund             ',13,10
db 9,9,'  3. RB Leipzig                 ',13,10
db 9,9,'  4. Union Berlin               ',13,10
db 9,9,'  Tekan enter untuk kembali ke menu $',13,10


belanda1 :
cls
        gotoxy 22, 12

        mov ah, 09h
        lea dx, belanda1_nama
        int 21h

        readkey
        jmp proses

belanda1_nama db 'klasemen Liga Belanda',9,10
db 9,9,'  1. Feyenord             ',13,10
db 9,9,'  2. PSV             ',13,10
db 9,9,'  3. Ajax                 ',13,10
db 9,9,'  4. AZ               ',13,10
db 9,9,'  Tekan enter untuk kembali ke menu $',13,10


karakter1 :
            cls
            gotoxy 22,12

            mov ah,2h
            mov dl,'A'
            mov cx,26
    k1        :
            int 21h
            inc dl
            loop k1
            readkey
            jmp proses
warna1  :
            cls
            gotoxy 22,12
           
            mov ah,9h
            mov bx,29h
            mov al,'Z'
            mov cx,26

    k2     :
            int 10h
            dec al
            inc bl
            loop k2
            readkey
            jmp proses

    rontok1 : jmp prontok

    prontok : jmp kalimat
                kal db 'SEKOLAH TINGGI CIPTA KARYA INFORMATIKA $'
                kal2 db 'Tekan Enter Untuk Kembali ke Menu $'

            kalimat :
            cls
            gotoxy 22,12

            mov ah,09
            lea dx,kal
            int 21h

            mov ax,0B800h
            mov es,ax

            mov bx,3998
            mov cx,25

        ulangY    :
        mov PosY,12
        push cx
        mov cx,80
ulangX     :
        cmp byte ptr es:[bx],33

        jb Tdk
        Geser PosY
Tdk        :
        sub bx,2
        loop ulangX
        pop cx
        loop ulangY

        gotoxy 22,12
        mov ah,09h
        lea dx,kal2
        int 21h

        readkey
        jmp proses

kalkulator1 :
jmp mulai

judul db ' Calculator ITBU'
tambah db ' [1] Tambah$'
kurang db ' [2] Kurang$'
kali db ' [3] Kali$'
bagi db ' [4] Bagi$'
keluark db ' Exit Program$'
sk2009 db ' ITBU$'
rule db ' RULE$'
up db ' = up$'
down db ' = down$'

pertambahan db 'Pertambahan$'
pengurangan db 'Pengurangan$'
perkalian db 'Perkalian$'
pembagian db 'Pembagian$'
back db ' Back$'
sd db '  = $'
tanda1 db '  + $'
tanda2 db '  - $'
tanda3 db '  x $'
tanda4 db '  : $'
hasil db ' hasil$'

pindah db 13,10,'$'

a db 0
b db 0
c db 1
d db 0
e db 0
f db 0
nilaix db 33
nilaiy db 6
nilaiy1 db 6
panah_atas equ 72
panah_bawah equ 80
enter_key equ 0dh

mulai:
cls
gotoxy 10,3
tampilk 201
ulangtampil 205, 33
tampilk 187
gotoxy 10,4
tampilk 186
gotoxy 44,4
tampilk 186
menu2 pindah
gotoxy 10,5
tampilk 204
ulangtampil 205, 20
tampilk 203
ulangtampil 205, 3
tampilk 203
ulangtampil 205, 8
tampilk 185
menu2 pindah
gotoxy 10,6
tampilk 186
ulangtampil 255, 20
tampilk 186
ulangtampil 255, 3
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,7
tampilk 186
ulangtampil 255, 20
tampilk 186
ulangtampil 255, 3
tampilk 204
ulangtampil 205, 8
tampilk 185
menu2 pindah
gotoxy 10,8
tampilk 186
ulangtampil 255, 20
tampilk 186
ulangtampil 255, 3
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,9
tampilk 186
ulangtampil 255, 20
tampilk 186
ulangtampil 255, 3
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,10
tampilk 204
ulangtampil 205, 20
tampilk 206
ulangtampil 205, 3
tampilk 206
ulangtampil 205, 8
tampilk 185
menu2 pindah
gotoxy 10,11
tampilk 186
ulangtampil 255, 20
tampilk 186
ulangtampil 255, 3
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,12
tampilk 200
ulangtampil 205, 20
tampilk 202
ulangtampil 205, 3
tampilk 202
ulangtampil 205, 8
tampilk 188
menu2 pindah
gotoxy 30, 9

tulis:
gotoxy 12, 4
menu2 judul
gotoxy 12, 6
menu2 tambah
gotoxy 12, 7
menu2 kurang
gotoxy 12, 8
menu2 kali
gotoxy 12, 9
menu2 bagi
gotoxy 12, 11
menu2 keluark
gotoxy 37, 11
menu2 sk2009
gotoxy 38, 6
menu2 rule
gotoxy 36, 8
tampilk 24
menu2 up
gotoxy 36, 9
tampilk 25
menu2 down
cmp c, 0
je input
dec c
jmp mulai
awal:
jmp mulai
input:
gotoxy nilaix, nilaiy
tampilk 17
readkey
cmp ah, panah_bawah
je bawahk
cmp ah, panah_atas
je atas
cmp al, enter_key
je setting
jmp input
bawahk:
cmp nilaiy, 9
je lompat1
cmp nilaiy, 11
je awal
inc nilaiy
jmp awal

lompat1:
mov nilaiy, 11
jmp awal

atas:
cmp nilaiy, 11
je lompat2
cmp nilaiy, 6
je awal
dec nilaiy
jmp awal

lompat2:
mov nilaiy, 9
jmp awal

setting:
cls
cmp nilaiy, 5
ja mulaihitung

mulaihitung:
cls
gotoxy 10,3
tampilk 201
ulangtampil 205, 15
tampilk 203
ulangtampil 205, 8
tampilk 187
gotoxy 10,4
tampilk 186
ulangtampil 255, 15
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,5
tampilk 204
ulangtampil 205, 6
tampilk 203
ulangtampil 205, 3
tampilk 203
ulangtampil 205, 4
tampilk 206
ulangtampil 205, 8
tampilk 185
menu2 pindah
gotoxy 10,6
tampilk 186
ulangtampil 255, 6
tampilk 186
ulangtampil 255, 3
tampilk 186
ulangtampil 255, 4
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,7
tampilk 186
ulangtampil 255, 6
tampilk 186
ulangtampil 255, 3
tampilk 186
ulangtampil 255, 4
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,8
tampilk 204
ulangtampil 205, 6
tampilk 202
ulangtampil 205, 3
tampilk 206
ulangtampil 205, 4
tampilk 206
ulangtampil 205, 8
tampilk 185
menu2 pindah
gotoxy 10,9
tampilk 186
ulangtampil 255, 10
tampilk 186
ulangtampil 255, 4
tampilk 186
ulangtampil 255, 8
tampilk 186
menu2 pindah
gotoxy 10,10
tampilk 200
ulangtampil 205, 10
tampilk 202
ulangtampil 205, 4
tampilk 202
ulangtampil 205, 8
tampilk 188
menu2 pindah
gotoxy 30, 9

cmp nilaiy, 6
je tambah1
cmp nilaiy, 7
je kurang1
cmp nilaiy, 8
je kali1
cmp nilaiy, 9
je bagi1
jmp proses
tambah1:
gotoxy 13, 4
menu2 pertambahan
gotoxy 11, 6
menu2 tanda1
jmp tulis1
kurang1:
gotoxy 13, 4
menu2 pengurangan
gotoxy 11, 6
menu2 tanda2
jmp tulis1
kali1:
gotoxy 13, 4
menu2 perkalian
gotoxy 11, 6
menu2 tanda3
jmp tulis1
bagi1:
gotoxy 13, 4
menu2 pembagian
gotoxy 11, 6
menu2 tanda4
jmp tulis1
tulis1:
mov nilaiy1, 6
gotoxy 11, 7
menu2 back
gotoxy 12, 9
menu2 hasil
gotoxy 28, 9
menu2 sk2009
gotoxy 28, 4
menu2 rule
gotoxy 27, 6
tampilk 24
menu2 up
gotoxy 27, 7
tampilk 25
menu2 down

input1:
gotoxy 19, nilaiy1
tampilk 17
baca1:
gotoxy 23, 6
readkey
cmp ah, panah_bawah
je bawah1
cmp ah, panah_atas
je atas1
cmp al, enter_key
je setting1
mov ah,02h
mov dl, al
int 21h
mov a,dl
loop baca1
jmp input1
atas1: jmp atas2
input2: jmp input1
bawah1:
gotoxy 19, nilaiy1
tampilk 255
cmp nilaiy1, 7
je input1
inc nilaiy1
jmp input1
atas2:
gotoxy 19, nilaiy1
tampilk 255
cmp nilaiy1, 6
je input2
dec nilaiy1
jmp input2

setting1:
cmp nilaiy1, 7
je kembali1
baca2:
gotoxy 23,7
readkey
cmp al, enter_key
je samadengan
mov ah,02h
mov dl, al
int 21h
mov b,dl
loop baca2
samadengan:
gotoxy 24, 9
tampilk 255
gotoxy 23, 9
mov ah, a
mov al, b
cmp nilaiy, 6
je sdt
cmp nilaiy, 7
je sdk
cmp nilaiy, 8
je sdka
cmp nilaiy, 9
je sdb
sdt:
add ah, al
mov dl,ah
sub dl, 48
jmp sadg
sdk:
sub ah, al
mov dl,ah
sub dl, 208
jmp sadg
kembali1: jmp kembali
sdka:
sub al, 48
sub ah, 48
mov cl, al
mov al, ah
mov ah, 0
sdka1:
add ah, al
loop sdka1
mov dl,ah
sub dl, 208
jmp sadg
sdb:
sub al, 48
sub ah, 48
mov e, 49
mov f, ah
mov ah, al
sdb1:
add ah, al
mov dl, e
cmp ah, f
ja sadg
inc e
jmp sdb1
sadg:
cmp dl, '9'
ja puluhan
cmp dl,'0'
jb min
mov ah,2
int 21h
jmp input2
min:
mov e, 49
ulang4:
inc dl
cmp dl, '0'
mov d, '-'
je tampil2
inc e
jmp ulang4
puluhan:
mov d, 49
ulang3: sub dl, 10
cmp dl, ':'
mov e, dl
jb tampil2
inc d
jmp ulang3
tampil2:
mov dl, d
mov ah,2
int 21h
mov dl, e
mov ah,2
int 21h
jmp input2
kembali:
mov c, 1
jmp mulai
readkey
jmp proses
    selesai :
            int 20h

end     tdata 

