#app.py 

from flask import Flask, session, render_template, url_for, redirect, request

app = Flask(__name__)

app.secret_key = 'chave_secreta'

@app.route('/')
def index():
    if 'nome' in session:
        return render_template('index.html', nome=session['nome'])
    else:
        return redirect(url_for('login'))  # ← CORRIGIDO

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        nome = request.form['nome']
        session['nome'] = nome
        return redirect(url_for('index'))  # ← CORRIGIDO
    else:
        return render_template('login.html')  # ← CORRIGIDO

@app.route('/logout')
def logout():
    session.pop('nome', None)
    return redirect(url_for('login'))

if __name__ == '__main__':
    app.run(debug=True)


#login.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Index</title>
</head>
<body>
    <h2>Digite seu nome:</h2>
    <form action="" method="POST">
        <input type="text" name="nome" required>
        <input type="submit" value="Entrar">
    </form>
</body>
</html>

#index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    <h2>Olá, {{ nome }}}</h2>
    <a href="{{url_for( 'logout' )}}">Sair</a>
</body>
</html>
