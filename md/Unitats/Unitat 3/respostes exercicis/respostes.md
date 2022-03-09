1. L'endpoint de consulta d'usuari per id és vulnerable a atacs d'injecció d'SQL? Demostra la teua afirmació.
    
    - És vulnerable. Amb el payload: 1%20UNION%20SELECT%20*%20FROM%20user%20WHERE%20ID%20=%203%20order%20by%20id%20desc obtenim qualsevol,usuari de qualsevol id.
    http://localhost:9000/api/user/1%20UNION%20SELECT%20*%20FROM%20user%20WHERE%20ID%20=%203%20order%20by%20id%20desc
    Obtenim l'usuari amb id = 3
    - Per mitigar esta vulnerabilitat utilitzem la parametrització (SQL parameter binding):
    Mirar /api/user2/

2. Utilitza Postman per a insertar un usuari.
    [Postman](../respostes%20exercicis/post-usuari.png)

3. Utilitza curl per a insertar un usuari.
    curl -d "name=test&email=test%40example.com&password=test123" -X POST http://localhost:9000/api/user/

4. Veus alguna configuració dèbil en l'endpoint de creació d'usuari? Si és el cas, corregix-la.
    // Afegim el mòdul md5
    var md5 = require("md5")
    // Encriptem la contrasenya abans d'insertar-la
    password : md5(req.body.password)

5. Prova l'API d'actualització d'usuaris utilitzant postman. Canvia noms, correus i contrasenyes.
   
6. Prova l'API d'actualització d'usuaris utilitzant curl. Canvia noms, correus i contrasenyes.
   curl -X PATCH -d "email=user@example1.com" http://localhost:9000/api/user/2

7. Construix l'endpoint d'eliminació d'usuaris. Utilitza el mètode delete d'Express.
    app.delete("/api/user/:id", (req, res, next) => {
        db.run(
            'DELETE FROM user WHERE id = ?',
            req.params.id,
            function (err, result) {
                if (err){
                    res.status(400).json({"error": res.message})
                    return;
                }
                res.json({"message":"deleted", changes: this.changes})
        });
    })

8. Fes proves amb postman i amb curl.
   curl -X "DELETE" http://localhost:8000/api/user/2
   
9.  L'endpoint d'eliminació d'usuaris que has implementat és vulnerable a atacs d'injecció d'SQL? Comprova-ho.