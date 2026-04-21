# Jenkins Install on Ubuntu

## Template hien co

- `install-jenkins.sh.example`
  Bash script de cai Jenkins tren Ubuntu bang package chinh thuc cua Jenkins va Java 21.

## Khi nao nen dung

- Can bootstrap nhanh Jenkins controller tren server Ubuntu
- Muon luu script cai dat ngay trong nhom `templates/jenkins/` de de tim
- Dang dung Jenkins package repository chinh thuc

## Can doi gi truoc khi dung

- Chay script bang user co quyen `sudo` hoac root
- Kiem tra lai version Java va Jenkins repository key truoc khi chay tren moi truong production
- Neu server khong dung `ufw`, hay sua hoac bo dong mo cong trong script

## Luu y

- File nay co noi dung giong script trong `templates/shared/docker/install/ubuntu/install-jenkins.sh.example`
- Ban trong `templates/jenkins/` duoc dat de nguoi tim tai nguyen Jenkins co the thay ngay trong cung mot nhom
