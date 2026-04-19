# Shared Docker Templates for Java

## Template hien co

- `maven-jar-openjdk8-jre-alpine.Dockerfile.example`
  Multi-stage Dockerfile cho du an Java build bang Maven, tao JAR va chay bang Java 8 tren Alpine voi non-root user.

## Khi nao nen dung

- Du an build bang Maven
- Artifact dau ra la runnable JAR
- Can mot Dockerfile don gian de dong goi va chay tren server hoac VPS
- Ung dung van chay tot voi Java 8

## Can doi gi truoc khi dung

- Doi ten JAR trong lenh `COPY --from=build` cho dung voi artifact thuc te
- Doi user runtime neu ban muon dat theo ten he thong cua tung du an
- Doi `EXPOSE 8080` neu ung dung nghe o cong khac
- Neu du an can profile, Maven Wrapper, hoac build command khac, cap nhat lenh `mvn install -DskipTests=true`

## Luu y

- Template nay giu cach build giong mau goc bang `mvn install -DskipTests=true` de phu hop voi cac du an da dung quy trinh nay.
- Runtime image dung `openjdk8-jre-base` de gon hon so voi cai full JDK, va goi truc tiep binary Java cua Alpine package de tranh phu thuoc vao PATH.
