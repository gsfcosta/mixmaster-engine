MixMaster Engine

Esta biblioteca tem como objetivo facilitar e gerencia do banco de dados do mixmaster (MMORPG)

Requisitos:
python 3.9+

Instalar
pip3 install mixmaster-engine

### Configurar Banco de Dados:
Em seu projeto crie o arquivo "settings.toml" <p>
Cole a configuracao abaixo:
```
[default]
host = "127.0.0.1"
user = "root"
password = "password"
porta = 3306
players = "Member"
game = "gamedata"
data = "S_Data"
log = "LogDB"
gametail = "Web_Account"
```
### Configurar Engine:

```
import MixMasterEngine

sdata = MixMasterEngine.data()
```

Conecta ao engine do banco de dados S_Data <p>
Possível fazer pesquisa de varias tabelas, como na s_itens, s_monster, s_npc etc<p>
Total de 30 funções get<p>
exemplo de uso:
```
items = sdata.get_all_items()
for item in items: # Faz um loop em todos os itens da s_item
print(f"{item.idx}: {item.name}") # Imprime o id e nome de todos os itens
  
item = sdata.get_item("Helmet") # Pode ser tanto id do item quanto o nome dele
print(f"{item.name}: {item.idx}") # Imprime nome e id do item Helmet
```
---
```player = MixMasterEngine.profile("username")```<p>
Carrega todas as informação da conta, como email, e os chars associados a ela <p>
exemplo de uso:
```
print(player.Email) # Imprime Email da conta
print(len(player.heros)) # Imprime quanto chars a conta possui
for hero in player.heros: # Faz um loop nos chars
  print(hero.name) # imprime o nome da cada char
```
---
```players = MixMasterEngine.players() ```<p>
Conecta ao engine do banco de dados Member.Players <p>
Total de 3 funções get, 1 set, 1 add e 1 del <p>
exemplo de uso:
```
search = players.get_player_by_email("example@example.com") # Pesquisa pelo usuário com o email example@example.com
print(search.Name) # Imprime o nome do jogador
players.set_player("user_test", "email", "new.example@example.com") # Altera o email do usuário user_test para new.example@example.com
```
---
```game = MixMasterEngine.game() ```<p>
Conecta ao engine do banco de dados gamedata <p>
Total de 77 funções get, 16 clean, 14 set, 2 add e 2 del <p>
exemplo de uso:
```
mixlog = game.get_mix_log_by_name("nick")
for mix in mixlog: # Faz um loop em todos os mix feitos do char
  print(f"Hench {mix.type}: {mix.SuccessCount}x") # imprime quais os ids de henchs ja mixados e quantas vezes obteve sucesso naquele hench.

game.del_hero_by_name("nick") # Remove o char da conta (remove também todos os itens, henchs, quests etc)
```
---
```loginLog = MixMasterEngine.loginLog() ```<p>
Conecta ao engine do banco de dados LogDB.LoginLog <p>
Total de 4 funções get <p>
exemplo de uso:
```
logs = loginlog.get_log_login_by_user("nick")
for log in logs: # Faz um loop em todos os logins feitos do char
  print(f"IP {log.IP}, LoginTime {log.LoginTime}") # imprime o ip e timestamp da data de login feito
```
---
```gametail = MixMasterEngine.gametail() ```<p>
Conecta ao engine do banco de dados Web_Account <p>
Possivel usar tanto com GameTail quanto com GameTail_Event <p>
Total de 6 funções get e 2 add <p>
exemplo de uso:
```
gametail.add_gametail_event("player1", 3, 5, 1, "30 cash") # adiciona o item id 5 (1x) para a conta player1 (id 3), com a info de 30 cash.
```
---
Tratamento de erros: <p>
Cada função retorna um objeto com as informações solicitadas ou boolean (True ou False), porém ao encontrar erros nos parâmetros passados para a função ou não encontrar a informação desejada, será retornado um Warning, que deverá ser tratado com try/Except. <p>
Exemplo:
```
game = MixMasterEngine.game()
try:
  hero = game.get_hero_by_name("legendario") # Busca o herói legendario
except Warning as error: # Caso traga um erro Warning, deverá:
  print(f"Erro: {error}") # Imprimir o erro na tela (pode ser que não tenha encontrado ou os parâmetro passados não coincidem com o solicitado pela função)
```
Neste caso retornou: <p>
```Erro: Hero not found```

Forçando parâmetro incorreto
```
try:
  hero = game.get_hero_by_name(True) # Busca um valor boolean (porém não é permitido)
except Warning as error:
  print(f"Erro: {error}")
```
Neste caso retornou: <p>
```Erro: Invalid values, value must be number or string```

Explicação das funções:
```
get - Busca informações no banco de dados
set - Alteração de informações ja existentes no banco de dados
add - Adiciona informações no banco de dados
del - Remove informações no banco de dados
clean - Limpa todas as linhas relacionadas ao herói que esta sendo removido
```
Lembrando que muitas dessas funções estão em fase de testes, por isso indico cuidado ao usar as funções set, add, del e clean. <p>
Qualquer bug informar para que possa ser atualizado
