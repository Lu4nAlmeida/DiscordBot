import discord
import random
from discord.ext import commands, tasks
import os
from random import choice
import json
from PIL import Image
from io import BytesIO
import youtube_dl
import aiohttp
from googleapiclient.discovery import build
from gtts import gTTS

def get_prefix(client, message):
    with open('Prefixes.json', 'r') as f:
        prefixes = json.load(f)
    return prefixes[str(message.guild.id)]

language = 'en'
lingua = 'pt'
client = commands.Bot(command_prefix = get_prefix)
status = ['Meu dono é pika', 'teu pai na cama', 'TA CÔNCAVO JOAQUIM!', '-se do Prédio']
client.ses = aiohttp.ClientSession()
api_key = "KEY"

@client.event
async def on_ready():
    change_status.start()
    print("Pai ta on!")

@client.event
async def on_guild_join(guild):
    with open('Prefixes.json', 'r') as f:
        prefixes = json.load(f)
    prefixes[str(guild.id)] = '!'

    with open('prefixes.json', 'w') as f:
        json.dump(prefixes, f, indent=4)

@client.event
async def on_guild_remove(guild):
    with open('Prefixes.json', 'r') as f:
        prefixes = json.load(f)
    prefixes.pop(str(guild.id))

    with open('prefixes.json', 'w') as f:
        json.dump(prefixes, f, indent=4)

@tasks.loop(seconds=20)
async def change_status():
    await client.change_presence(activity=discord.Game(choice(status)))


@client.command(name='image', help='Pesquisa uma imagem no google', aliases=['imagem','pesquisar', 'search'])
async def image(ctx, *, pesquisa):
    ran = random.randint(0,9)
    resource = build("customsearch","v1",developerKey=api_key).cse()
    result = resource.list(
        q=f"{pesquisa}", cx="6cda7737e7eabccdf",searchType="image"
    ).execute()
    url = result["items"][ran]["link"]
    embed1 = discord.Embed(title=f'Aqui está sua imagem ({pesquisa.title()})')
    embed1.set_image(url=url)
    await ctx.send(embed=embed1)


@client.command(name='speak', help='Faz o bot falar algo em inglês com a voz do google tradutor', aliases=['say'])
async def speak(ctx, *, texto):
    output = gTTS(text=texto, lang=language, slow=False)
    output.save("output.mp3")
    await ctx.send(file=discord.File(r'output.mp3'))

@client.command(name='falar', help='Faz o bot falar algo em português com a voz do google tradutor')
async def falar(ctx, *, texto):
    output = gTTS(text=texto, lang=lingua, slow=False)
    output.save("output.mp3")
    await ctx.send(file=discord.File(r'output.mp3'))

@client.command(name='novidades', help='Mostra as novidades da última atualização do bot')
async def novidades(ctx):
    await ctx.send('**Novidades do Patch 1.3.5**\n\n-Novo comando !falar e !speak. Use !help *comando* para saber mais\n-Novo comando "!image *pesquisa*". Use !help image para saber mais\n-Novo comando "!help *comando*" adicionado, mostra informações sobre aquele comando. Para ver a lista de comandos digite "!help"\n-Novos Status que mudam a cada 20 segundos\n-Prefixo consertado, agora é possível alterar o prefixo\n-Comando !ping agora mostra o ping do bot\n-Comando !clear adicionado\n-Mais 5 outras correções e adições pequenas\n-Comando !dice pode escolher a quantidade de lados do dado\n-Comando !clear pode escolher a quantidade de mensagens a serem apagadas\n-Comando !play funcional\n-Novas frases e comandos pequenos')


@client.command(name='prefixchange', help='Muda o prefixo do bot ', aliases=['prefixomudar', 'PrefixoMudar', 'PrefixChange', 'setprefix'])
async def prefixchange(ctx, *, prefix):
    with open('Prefixes.json', 'r') as f:
        prefixes = json.load(f)
    prefixes[str(ctx.guild.id)] = prefix

    with open('prefixes.json', 'w') as f:
        json.dump(prefixes, f, indent=4)
    await ctx.send(f'Prefixo mudado para "{prefix}"')

@client.command(name='wanted', help='Faça seus amigos serem procurados pela polícia do velho oeste!', aliases=['procurado'])
async def wanted(ctx, user: discord.Member = None):
    if user == None:
        user = ctx.author
    wanted = Image.open("wanted.jpg")

    asset =  user.avatar_url_as(size = 128)

    data = BytesIO(await asset.read())
    pfp =  Image.open(data)

    pfp = pfp.resize((85, 85))

    wanted.paste(pfp, (51,88))

    wanted.save("profile.jpg")

    await ctx.send(file = discord.File("profile.jpg"))

@client.command(name='prefix', help='Mostra o prefixo atual do bot', aliases=['prefixo', 'Prefixo', 'Prefix'])
async def prefix(ctx):

    await ctx.send(f'O prefixo atual do bot é "{prefix}", para mudá-lo digite "{prefix}prefixchange *Novo Prefixo*" ')

@client.command(name='dice', help='Escolhe um número aleatório entre 1 e 6 (D6), caso um número seja colocado após o comando, escolhe um número aleatório entre 1 e o número escolhido', aliases = ['d', 'D'])
async def dice(ctx, alcance=6):

    dado = random.randrange(1,alcance + 1)

    await ctx.send(f'Você rolou {alcance} e caiu ' + str(dado))

@client.command(name='quem', help='Faça uma pergunta começando com "quem" para o bot,  por exemplo "!quem te criou?"', aliases=['Quem'])
async def quem(ctx, *, frase):
    if frase == 'te criou?' or frase == 'te criou':
        await ctx.send('Esse lindo me programou do 0 https://www.youtube.com/channel/UCnDJkmdj_q86j1Zl1fziNxw')
    elif frase == 'é Mário?':
        await ctx.send('Aquele que te comeu atrás do Armário')
    else:
        patadas = ['Sei lá porra, eu tenho cara de quem sabe?',
               'Tua mãe.',
               'teu pai.',
               'tua irmã',
               'Eu',
               'Sei não mano, pergunta pro google',
               'sim!',
               'Ningúem.',
               'Toma no cu, se acha que eu vou saber?',
               'Não sei.',
               'Não vou te dizer.',
               'Esqueci -_-',
               'Alguém.',
               'Disse alguma coisa? Eu não tava prestando atenção.',
               'Concentre-se e pergunte novamente.',
               'O Mário...']

        await ctx.send(f'{random.choice(patadas)}')
@client.command(name='play', help='Faz o bot tocar uma url do youtube, por exemplo "!play https://youtu.be/wvejNEkuLIY"', aliases = ['p', 'P'])
async def play(ctx, url : str):
    song_there = os.path.isfile("song.mp3")
    try:
        if song_there:
            os.remove("song.mp3")
    except PermissionError:
            await ctx.send('Esperando música atual terminar, caso queira pará-la use o comando !stop')
            return

    voiceChannel = discord.utils.get(ctx.guild.voice_channels, name='geral')
    await voiceChannel.connect()
    voice = discord.utils.get(client.voice_clients, guild=ctx.guild)

    ydl_opts = {
        'format': 'bestaudio/best',
        'postprocessors': [{
            'key': 'FFmpegExtractAudio',
            'preferredcodec': 'mp3',
            'preferredquality': '192',
        }],
    }
    with youtube_dl.YoutubeDL(ydl_opts) as ydl:
        ydl.download([url])
        for file in os.listdir("./"):
            if file.endswith(".mp3"):
                os.rename(file, "song.mp3")
            voice.play(discord.FFmpegPCMAudio("song.mp3"))


@client.command(name='leave', help='Faz o bot sair da call')
async def leave(ctx):
    voice = discord.utils.get(client.voice_clients, guild=ctx.guild)
    if voice.is_connected():
        await voice.disconnect()
    else:
        await ctx.send('o bot não está conectado a um canal de voz')

@client.command(name='stop', help='Faz o bot parar de tocar sem sair da call')
async def stop(ctx):
    voice = discord.utils.get(client.voice_clients, guild=ctx.guild)
    voice.stop()

@client.command(name='ping', help='Checa o ping do bot')
async def ping(ctx):
    await ctx.send(f'pong! {round(client.latency * 1000)}ms')

@client.command(name='puta', help='Descubra você mesmo')
async def puta(ctx):
    await ctx.send('tua mãe')

@client.command(name='sabedoria', help='O bot fala uma frase motivacional de biscoito da sorte')
async def sabedoria(ctx):
    responses = ['A vida trará coisas boas se tiveres paciência.',
'Demonstre amor e alegria em todas as oportunidades e verás que a paz nasce dentro de você.',
'Não compense na ira o que lhe falta na razão.',
'Defeitos e virtudes são apenas dois lados da mesma moeda.',
'A maior de todas as torres começa no solo.',
'Não há que ser forte. Há que ser flexível.',
'Gente todo dia arruma os cabelos, por que não o coração?',
'Há três coisas que jamais voltam; a flecha lançada, a palavra dita e a oportunidade perdida.',
'A juventude não é uma época da vida, é um estado de espírito.',
'Podemos escolher o que semear, mas somos obrigados a colher o que plantamos.',
'Dê toda a atenção para a formação dos teus filhos, sobretudo por exemplos de tua própria vida.',
'Siga os bons e aprenda com eles.',
'Não importa o tamanho da montanha, ela não pode tapar o sol.',
'O bom-senso vai mais longe do que muito conhecimento.',
'Quem quer colher rosas deve suportar os espinhos.',
'São os nossos amigos que nos ensinam as mais valiosas lições.',
'Uma iniciativa mal-sucedida não significa o final de tudo. Sempre existe uma nova oportunidade.',
'Aquele que se importa com o sentimento dos outros, não é um tolo.',
'A adversidade é um espelho que reflete o verdadeiro eu.',
'Lamentar aquilo que não temos é desperdiçar aquilo que já possuímos.',
'Uma bela flor é incompleta sem suas folhas.',
'Sem o fogo do entusiasmo, não há o calor da vitória.',
'Não há melhor negócio que a vida. A gente há obtém a troco de nada.',
'O riso é a menor distância entre duas pessoas.',
'Você é jovem apenas uma vez. Depois precisa inventar outra desculpa.',
'É mais fácil conseguir o perdão do que a permissão.',
'Os defeitos são mais fortes quando o amor é fraco.',
'Amizade e Amor são coisas que podem virar uma só num piscar de olhos.',
'Surpreender e ser surpreendido é o segredo do amor.',
'Faça pequenas coisas agora e maiores coisas lhe serão confiadas cada dia.',
'Todo mundo é capaz de denominar uma dor, exceto quem a sente.',
'A paciência na adversidade é o sinal de um coração sensível.',
'A sorte favorece a mente bem preparada.',
'Sua visão se tornará clara apenas quando você puder olhar dentro de seu coração.',
'Quem olha para fora sonha; quem olha para dentro acorda.',
'As pessoas se esquecerão do que você disse e do que você fez… mas nunca se esquecerão de como você as fez sentir.',
'Espere pelo mais sábio dos conselhos: o tempo.',
'Todas as coisas são difíceis antes de se tornarem fáceis.',
'Você pode encontrar a si mesmo fazendo ou dizendo coisas que você nunca imaginou possíveis.',
'Se você se sente só é porque construiu muros ao invés de pontes.',
'Vencer é 90 por cento suor e 40 por cento técnica.',
'O amor está mais próximo do que você imagina.',
'A vida coloca em nossa frente opções.',
'Você é do tamanho do seu sonho.',
'Pare de procurar eternamente; a felicidade está bem ao seu lado.',
'Conhecimento é a única virtude e ignorância é o único vício.',
'O nosso primeiro e último amor é… o amor-próprio.',
'Deixe de lado as preocupações e seja feliz.',
'A vontade das pessoas é a melhor das leis.',
'Nós somos o que pensamos.',
'A maior barreira para o sucesso é o medo do fracasso.',
'O pessimista vê a dificuldade em cada oportunidade; O otimista vê a oportunidade em cada dificuldade.',
'Muitas das grandes realizações do mundo foram feitas por homens cansados e desanimados que continuaram trabalhando.',
'O insucesso é apenas uma oportunidade para recomeçar de novo com mais inteligência.',
'O futuro pertence àqueles que acreditam na beleza de seus sonhos.',
'Coragem é a resistência ao medo, domínio do medo, e não a ausência do medo.',
'O verdadeiro homem mede a sua força, quando se defronta com o obstáculo.',
'Você precisa fazer aquilo que pensa que não é capaz de fazer.',
'Quem quer vencer um obstáculo deve armar-se da força do leão e da prudência da serpente.',
'A adversidade desperta em nós capacidades que, em circunstâncias favoráveis, teriam ficado adormecidas.',
'A vida é para quem topa qualquer parada. Não para quem pára em qualquer topada.',
'Motivação não é sinônimo de transformação, mas um passo em sua direção.',
'O que empobrece o ser humano, não é a falta de dinheiro, mais sim, a falta de fé,motivação e criatividade.',
'Inspiração vem dos outros. Motivação vem de dentro de nós.',
'Não acredite mais em pessoas especiais, mas em momentos especiais com pessoas habituais.',
'“A nossa vida tem 4 sentidos…amar, sofrer, lutar e vencer. Ame muito, sofra pouco,lute bastante e vença sempre!”',
'Nada é por acaso…Acredite em seus sonhos e nos seus potenciais….Na vida tudo se supera..',
'Acredite em milagres, mas não dependa deles.',
'Você sempre será a sua melhor companhia!',
'Realize o óbvio, pense no improvável e conquiste o impossível Latumia. (W.J.F.)']

    await ctx.send(random.choice(responses))

@client.command(name='clear', help='O bot apaga as últimas 5 mensagens')
async def clear(ctx, amount=6):

    await ctx.channel.purge(limit=amount)

@client.command(name='_8ball', help='Faça uma pergunta de sim ou não, exemplo.: "!8ball eu vou morrer? (Não precisa escrever o "_" pode só colocar !8ball)"', aliases=['8ball'])
async def _8ball(ctx, *, pergunta):
    res =       ['Certamente.',
                 'É decididamente um sim.',
                 'Sem dúvida.',
                 'Sim, definitivamente.',
                 'Você pode contar com isso.',
                 'A meu ver, sim.',
                 'Provavelmente.',
                 'Me parece um bom sim.',
                 'Sinais apontam que sim.',
                 'Resposta nebulosa, tente novamente.',
                 'Pergunte novamente mais tarde.',
                 'Melhor não te dizer agora.',
                 'Não posso prever agora..',
                 'Concentre-se e pergunte novamente.',
                 'Não conte com isso.',
                 'Minha resposta é não.',
                 'Minhas fontes dizem não.',
                 'Me parece um bom não.',
                 'Muito duvidoso.']

    await ctx.send(f'Pergunta: {pergunta}\nResposta: {random.choice(res)}')
@client.command(name='piada', help='Não use esse comando pelo bem da sua sanidade mental')
async def piada(ctx):
    responses = ['Eu morava em uma ilha e mudei para outra. Não foi um trocadilho, mas foi uma trocadILHA. Agradeço a oportunidade',
                 'Sabe a banda que te avisa quando tá passando vergonha?\nR.: Paramore',
                 'Tinham dois patos conversando. Um falou "quack", aí o outro respondeu:\n— Nossa eu ia falar isso agora!',
                 'Qual é o aracnídeo que mais roda?\nR.: O escorPIÃO',
                 '— Ei, sabe qual país que não aceita táxi?\n— Não, qual?\n— Uberlândia',
                 'Qual foi a primeira vez que os americanos comeram carne?\nQuando chegou Cristovão COM LOMBO (Colombo)',
                 'Qual é o ator que está achando esse ano ruim?\nR.: Keanu Reeves (Que ano horrivis/horrivel)',
                 'Um senhor chega a um restaurante e chama o garçom:\n— Olá, meu nome é Djalma e eu queria um suco de laranja.\nO garçom, sem entender, pergunta:\n— De que?\n— Djalma!',
                 'Sabe o que o rato diz quando se queima?\n— Nossa mickey mei! (Nossa me queimei)',
                 'Qual o cereal favorito do vampiro?\nAveia!',
                 'Antes da chuva eu lutava capoeira\nAgora depois da chuva eu luto combarro!',
                 'Um mineiro e um gringo se envolveram em um acidente de carro e o gringo foi falar com o outro rapaz:\n— Hello!\nO mineiro irritado responde:\n— Relô nada. Amassô foi tudo!',
                 'O que o sal falou para a batata frita?\nÉ nois na frita (é nois na fita)',
                 'Uma família ficou perdida no deserto por 80 dias. Eles encontraram um rio e todos tomaram banho, menos a avó. Qual o nome do filme?\nR: A Vó Ta Imunda a 80 Dias. (A Volta ao Mundo em 80 Dias)',
                 'Duas formigas japonesas se encontraram:\n— Oi, como você se chama?\n— Fu\n— Fu o que?\n— Fumiga. E você?\n— Ota\n— Ota o que?\n— Ota fumiga!',
                 'Por que a galinha foi na igreja?\nPorque ela queria ver a Missa do Galo',
                 'O que acontece quando chove na Inglaterra?\nEla vira Inglalama!',
                 'O que o pagodeiro foi fazer na igreja?\nEle foi cantar pá god! (pagode)',
                 '— Oi, meu nome é Jaqueline, eu tenho 12 anos e já namoro.\n— Já o que???\n— Queline',
                 'O que o pintinho falou para a sua mãe?\n— PIU!']

    await ctx.send(random.choice(responses))



client.run('TOKEN')
