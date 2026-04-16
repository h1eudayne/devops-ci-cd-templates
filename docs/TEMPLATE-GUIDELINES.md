# Template Guidelines

## 1. Khong gan ten project cu the vao ten file

Nen dat theo use-case:

- Dung: `maven-jar-server-tag.yml`
- Khong nen: `shoe-shopping-cart.yml`

## 2. Xep dung delivery model truoc khi luu template

- `continuous-integration`
  Chi build, test, lint, scan, package.

- `continuous-delivery`
  Tao artifact san sang release hoac con manual gate truoc production.

- `continuous-deployment`
  Pipeline tu deploy vao moi truong dich ma khong can approve step trong pipeline.

Neu chua chac, uu tien xep vao `continuous-delivery`, sau do ghi note ro trong README.

## 3. Tach bien can tuy bien len dau file

Tat ca gia tri de doi theo tung project nen nam trong `variables`, `env` hoac phan dau cua pipeline.

Nen uu tien cac nhom bien:

- thong tin ung dung
- thong tin artifact
- thong tin deploy target
- trigger rule

## 4. Ghi ro assumption

Neu template yeu cau:

- runner co `mvn`
- server co Java
- co quyen `sudo`
- deploy bang `nohup`

thi phai ghi ro trong README cung nhom template.

## 5. Khong commit thong tin nhay cam

Khong de truc tiep:

- password
- private key
- host noi bo neu khong can
- access token

Chi de placeholder hoac ten bien moi truong.

## 6. Them notes rollback va log

Template thuoc nhom delivery hoac deployment nen co toi thieu mot trong cac cach debug sau:

- in log service
- chi duong dan log
- rollback artifact truoc do
- restart command

## 7. Danh dau do truong thanh cua template

Co the su dung `notes` trong catalog de danh dau:

- `ready`: da dung on dinh o du an that
- `adapted`: da chuan hoa lai tu du an cu
- `draft`: moi tao khung, chua verify

## 8. Mau checklist truoc khi commit template moi

- Da bo secret
- Da xep dung `delivery_model`
- Da doi ten file theo dung scenario
- Da them entry vao catalog
- Da ghi assumption quan trong
- Da test lai syntax YAML
