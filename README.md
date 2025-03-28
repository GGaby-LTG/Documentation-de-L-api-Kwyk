# Documentation-api-Kwyk




    Token csfr:
        Trouvé dans le html de la page dans un input avec le nom csfrmiddlewaretoken, il faut l'extraire avec un scraper avant de pouvoir login example sous python:
        
        url = "https://www.kwyk.fr/accounts/login/"
        response = session.get(url)

        soup = BeautifulSoup(response.text, 'html.parser')

        csrf_token = soup.find('input', {'name': 'csrfmiddlewaretoken'})['value']



    Login : 
        Url : https://www.kwyk.fr/accounts/login/

        headers {
            'Content-Type': 'application/x-www-form-urlencoded',
            'Referer': url,
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36',
        }

        paloads{
            'login': 'email',
            'password': 'mot de passe',
            'csrfmiddlewaretoken': csrf_token,
        }"

        Il faudra récupérer un cookie dans la réponse, ce cookie est utilisé pour toute connection à un page non publique du site, il marche pendant
        plusieurs heures avant de le refresh. Toute nouvelle connection crée un nouveau cookie et invalide l'ancien.


    Les headers sont maintenant:
         {
            Content-Type': 'application/x-www-form-urlencoded',
            'Referer': url,  # Ensure the Referer header is set to the login page URL
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36',
            'Cookie': Cookie
         }   

    

        UUID :
            L'ID est necessaire pour acceder aux informations de l'élève. Elle est assé difficile à obtenir car il faut
            la scraper d'une page (n'importe laquelle).

            Pour la page devoir il faut scraper:
                <li><a class="header_button" href="/bilan/Classe/UUID/student/">Bilan</a></li>
            
            voila un example python:

            response = session.post(url,headers=headers)
            soup = BeautifulSoup(response.text, 'html.parser')
            a_tag = soup.findAll( class_='header_button', href=True)
            for x in range(len(a_tag)):
                if a_tag[x] and 'href' in a_tag[x].attrs:
                    href = a_tag[x]['href']
                    if "bilan" in str(href):
                        number = href.split("/")[3]
                        return number


            Ce nombre est utilisé pour les URLs des pages spécifiques à l'utilisateur et l'immense majorité des pages spécifiques à l'API
            il est permanent et peut être stocké et réutilisé.

