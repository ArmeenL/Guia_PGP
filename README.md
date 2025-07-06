// ============ // **PGP** //  ============= //

Com PGP, você pode criar uma chave privada que pertencerá somente à você, e uma chave pública, que você pode distribuir para qualquer pessoa.
Como você possui a chave privada, você é o único que pode descriptografar as mensagens da chave pública. Assim permitindo uma troca de informações segura.  


- Como usar PGP

    É um desafio decorar todos os comandos do GPG. Então eu decidi listar aqui os mais importantes:

    
    **— Gerar um par de chaves PGP:**
    
    gpg --full-generate-key
    
    **— Listar suas chaves públicas:**
    
    gpg --list-keys
    
    **— Listar suas chaves privadas:**
    
    gpg --list-secret-keys
    
    **— Listar o ID das chaves salvas:**
    
    gpg --fingerprint
    
    ---
    
    **— Exportar sua chave pública**
    
    gpg --export -a <SEU_ID> > minha-chave-publica.asc
    
    **— Importar chave pública de outra pessoa**
    
    gpg --import chave-pub.txt
    
    **— Exportar sua chave privada (COM CUIDADO! não compartilhe com ninguém)** 
    
    gpg --export-secret-keys -a <SEU_ID> > minha-chave-privada.asc
    
    ---
    
    **— Gerar certificado de revogação (Caso você perca sua chave privada. Você pode invalidá-la com esse certificado, por questões de segurança)**
    
    gpg --output revocation.crt --gen-revoke <SEU_ID>
    
    **— Usar certificado para revogar uma chave**
    
    gpg --import revocation.crt
    
    **— Enviar chave (ou revogação) para servidor público. Assim, sua chave será oficialmente invalidada**
    
    gpg --send-keys <SEU_ID>
    
    ---
    
    **— Criptografar e enviar mensagem para alguém (só funciona com a chave pública do destinatário importada)**
    
    gpg -e -r <ID_DESTINATARIO> mensagem.txt
    
    **— Descriptografar mensagem**
    
    gpg -d mensagem.txt.gpg
    
    ---
    
    **— Assinar um arquivo, assim provando que você é realmente o dono desse arquivo**
    
    gpg --sign arquivo.txt
    
    **— Verificar assinatura de um arquivo**
    
    gpg --verify arquivo.txt.gpg
    

---

// ============ // **BACKUP** **//**  ============= //


Caso você perca sua chave privada, você nunca mais iria conseguir recuperá-la sem um sistema de backup. Por isso, é de extrema importância ter um sistema de backup.

Por questões de segurança, salve isso de forma offline, como em um pendrive por exemplo.


1- **Exporte sua chave pública, chave privada e certificado de revogação para uma pasta chamada backup**

2- **Empacote tudo em um arquivo.tar:** tar -cf pgp-backup.tar backup 

3- **Criptografar o arquivo.tar:** gpg --symmetric --cipher-algo AES256 pgp-backup.tar

4- Apagar os arquivos originais com segurança, seria perigoso deixá-los expostos:  

- shred -u pgp-backup.tar

5- Agora temos somente o arquivo.tar criptografado. Passe o arquivo para um pendrive.

---

// ============ // **RESTAURAR BACKUP NO FUTURO** // ============ //

Caso precise restaurar seus arquivos: pegue o seu arquivo de backup criptografado, e siga as próximas etapas:

1- **Descriptografar arquivo:** gpg -d pgp-backup.tar.gpg > pgp-backup.tar

2- **Extrair conteúdo do arquivo:** tar -xf pgp-backup.tar

3- **Importar novamente as chaves:**

- gpg --import private.asc
- gpg --import public.asc
- gpg --import revocation.crt   # só se quiser revogar

