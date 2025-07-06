## 🛡️ Guia Rápido de PGP (GnuPG)
Com o PGP (Pretty Good Privacy), você pode:

- Criar um par de chaves (privada e pública)
- Criptografar mensagens para garantir confidencialidade
- Assinar arquivos, provando autoria
- Compartilhar sua chave pública com segurança
- E revogar chaves em caso de perda ou comprometimento

Este guia contém comandos essenciais e boas práticas de PGP.

### 📘 Índice:

- 🔐 Comandos PGP Essenciais
- 💾 Backup Seguro
- 🧰 Restauração de Backup

---

## 🔐 Comandos PGP Essenciais
É um desafio decorar todos os comandos do GPG, então aqui vai um resumo prático:

### 📌 Criar e Gerenciar Chaves

```bash

#Gerar um novo par de chaves PGP:
gpg --full-generate-key

#Listar suas chaves públicas:
gpg --list-keys

#Listar suas chaves privadas:
gpg --list-secret-keys

#Ver o ID e a impressão digital das chaves:
gpg --fingerprint

---

### 📤 Exportar / Importar Chaves

#Exportar sua chave pública:
gpg --export -a <SEU_ID> > minha-chave-publica.asc

#Importar chave pública de outra pessoa:
gpg --import chave-pub.txt

#Exportar sua chave privada (⚠️ NÃO COMPARTILHE!):
gpg --export-secret-keys -a <SEU_ID> > minha-chave-privada.asc

---

### 🚫 Revogar uma Chave

#Gerar certificado de revogação:
gpg --output revocation.crt --gen-revoke <SEU_ID>

#Importar o certificado para revogar:
gpg --import revocation.crt

#Enviar chave (ou revogação) para servidor público:
gpg --send-keys <SEU_ID>

---

### ✉️ Criptografar / Descriptografar Mensagens

#Criptografar mensagem (precisa da chave pública do destinatário):
gpg -e -r <ID_DESTINATARIO> mensagem.txt

#Descriptografar mensagem:
gpg -d mensagem.txt.gpg

---

### 🖊️ Assinar e Verificar Arquivos

#Assinar um arquivo:
gpg --sign arquivo.txt

#Verificar assinatura de um arquivo:
gpg --verify arquivo.txt.gpg

```
---

### 💾 Backup Seguro


Se perder sua chave privada, você perde acesso a todos os seus dados criptografados. Por isso, é de extrema importância ter um sistema de backup.

Por segurança, armazene tudo em um dispositivo offline como um pendrive.

Passo-a-passo:
Exporte suas chaves e certificado para uma pasta chamada backup
(Ela deve conter: chave pública, chave privada e revocation.crt)

```bash

#Empacotar tudo:
tar -cf pgp-backup.tar backup

#Criptografar o pacote com senha:
gpg --symmetric --cipher-algo AES256 pgp-backup.tar

#Apagar o arquivo original com segurança:
shred -u pgp-backup.tar

#Mover o arquivo pgp-backup.tar.gpg para um pendrive.

```
---

### 🧰 Restauração de Backup


Para restaurar seu backup em outro sistema (ou após perda de dados):

```bash

#Descriptografar o arquivo:
gpg -d pgp-backup.tar.gpg > pgp-backup.tar

#Extrair o conteúdo:
tar -xf pgp-backup.tar

#Importar novamente as chaves:
gpg --import private.asc
gpg --import public.asc
gpg --import revocation.crt (opcional: se quiser revogar)
```
