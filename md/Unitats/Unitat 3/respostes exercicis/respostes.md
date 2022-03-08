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

5. 