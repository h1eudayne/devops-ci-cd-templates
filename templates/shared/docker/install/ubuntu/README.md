# Shared Docker Install Scripts for Ubuntu

## Template hien co

- `install-docker-and-compose.sh.example`
  Bash script de cai Docker Engine va standalone Docker Compose tren Ubuntu bang APT + binary download.

## Khi nao nen dung

- Server dich dung Ubuntu
- Can bootstrap nhanh Docker tren may moi
- Muon giu luon ca lenh `docker-compose` kieu cu de tuong thich voi script hien co

## Can doi gi truoc khi dung

- Chay script bang user co quyen `sudo`
- Kiem tra lai phien ban Ubuntu va repository Docker truoc khi chay tren moi truong production
- Neu he thong da chuyen sang `docker compose` plugin, co the doi script theo cach cai plugin chinh chu thay vi binary `docker-compose`

## Luu y

- Script nay giu nguyen flow tu mau goc de de copy dung nhanh.
- File duoc dat duoi dang `.example` de nhac nguoi dung review lai truoc khi chay tren server that.
