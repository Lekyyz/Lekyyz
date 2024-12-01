-- ══════════════════════════════════════
-- Essencial				
-- ══════════════════════════════════════
local Find = função(Tabela) para i,v em pares(Tabela ou {}) faça se typeof(v) == 'tabela' então retorne v fim; fim; fim
Opções locais = Find(({...})) ou {
	Atalho de teclado = 'Início',

	Idioma = {
		IU = 'pt-br',
		Palavras = 'pt-br'
	},

	Experimentos = { },

	Tempo = 0,5,
	Arco-íris = falso,
}
Versão local = '1.5'
local Pai = gethui() ou jogo:GetService('CoreGui');
local require = função(Nome)
	retornar loadstring(jogo:HttpGet(('https://raw.githubusercontent.com/Zv-yz/AutoJJs/main/%s.lua'):formato(Nome)))()
fim

-- ══════════════════════════════════════
-- Serviços				
-- ══════════════════════════════════════
TweenService local = jogo:GetService('TweenService')
Jogadores locais = jogo:GetService('Jogadores')
LP local = Jogadores.JogadorLocal

-- ══════════════════════════════════════
-- Módulos				
-- ══════════════════════════════════════
IU local = require("IU")
Notificação local = require("Notificação")

local Extenso = require("Módulos/Extenso")
local Caractere = require("Módulos/Caractere")
local RemoteChat = require("Módulos/RemoteChat")
Solicitação local = require("Módulos/Solicitação")

-- ══════════════════════════════════════
-- Constantes				
-- ══════════════════════════════════════
local Char = Caractere.novo(LP)
Elementos de interface do usuário locais = UI.UIElementos;

Encadeamento local;
local FinishedThread = falso;
local Alternado = falso;
Configurações locais = {
	Atalho de teclado = Opções.Atalho de teclado ou 'Home',
	Iniciado = falso,
	Salto = falso,
	Configuração = {
		Início = nulo,
		Fim = nulo,
		Prefixo = nulo,
	}
}

-- ══════════════════════════════════════
-- Funções				
-- ══════════════════════════════════════
função local ListenChange(Obj)
	se Obj:GetAttribute('OnlyNumber') então
		Obj:GetPropertyChangedSignal('Texto'):Conectar(função()
			Obj.Texto = Obj.Texto:gsub("[^%d]", "")
		fim)
	fim
	Obj.FocusLost:Connect(função()
		local CurrentText = Obj.Text
		se não for CurrentText ou string.match(CurrentText, "^%s*$") então retorne fim
		Configurações.Config[Obj.Parent.Name] = Obj.Text
	fim)
fim

função local EndThread(sucesso)
	se encadeamento então
		se não FinishedThread então task.cancel(Threading) fim
		Enfiamento = nulo
		FinishedThread = falso
		Configurações["Iniciado"] = falso
		
		se Notificação então
			Notificação:Notificar(sucesso e 6 ou 12, nulo, nulo, nulo)
		fim
	fim
fim

função local DoJJ(n, prefixo, salto)
	sucesso local, extenso = Extenso:Convert(n)
	prefixo local = prefixo e prefixo ou ''
	se sucesso então
		--> Em minha opinião, esse código ta horrível - Zv_yz
		se pular então Char:Jump() fim
		se table.find(Options.Experiments, 'hell_jacks_2024_02-dev') então
			para i = 1, #extenso do
				se pular então Char:Jump() fim
				RemoteChat:Enviar(('%s'):formato(extenso:sub(i, i)))
				tarefa.wait(Opções.Tempo)
			fim
			se pular então Char:Jump() fim -- lol por que 2
			RemoteChat:Enviar(('%s'):formato(extenso .. prefixo))
		outro
			RemoteChat:Enviar(('%s'):formato(extenso .. prefixo))
		fim
	fim
fim

função local StartThread()
	Configuração local = Configurações.Config;
	se não for Config["Start"] ou não Config["End"] então retorne end
	se Threading então EndThread(false) retorna fim
	se Notificação então Notificação:Notificar(5, nulo, nulo, nulo) fim
	Encadeamento = tarefa.spawn(função()
		para i = Config.Start, Config.End faça
			--> mano, esse código parece tão ruim :sob: - Zv_yz
			se table.find(Options.Experiments, 'hell_jacks_2024_02-dev') então
				DoJJ(i, Config["Prefixo"], Configurações["Saltar"])
			outro
				task.spawn(DoJJ, i, Config["Prefixo"], Configurações["Saltar"])
			fim
			se i ~= tonumber(Config.End) então task.wait(Options.Tempo) fim;
		fim
		FinishedThread = verdadeiro
		Fim do tópico (verdadeiro)
	fim)
fim

função local GetLanguage(Lang)
	Sucesso local, Resultado = pcall(function()
		retornar require(('I18N/%s'):formato(Lang))
	fim)
	se Sucesso então
		retornar Resultado
	fim
	retornar {}
fim

função local MigrateSettings()
	local Lang = Opções['Idioma'];
	Experimentos locais = Opções['Experimentos'];
	se não experimentos então
		Opções['Experimentos'] = {};
	fim
	se typeof(Lang) == 'string' então
		Opções['Idioma'] = { UI = Idioma, Palavras = Idioma };
	fim
fim

MigrarConfigurações()

-- ══════════════════════════════════════
-- Principal				
-- ══════════════════════════════════════
UI:SetVersion(Versão)
IU:SetLanguage(Options.Language.UI)
UI:Definir Arco-Íris(Opções.Arco-Íris)
UI:SetParent(Pai)

Extensão:SetLang(GetLanguage(Opções.Language.Words))

se Notificação então
	Notificação:SetParent(UI.getUI())
	Notificação:SetLang(GetLanguage(Options.Language.UI))
fim

para i,v em pares(UIElements["Box"]) faça
	OuvirMudar(v)
fim

UIElements["Círculo"].MouseButton1Click:Conectar(função()
	se alternado então
		Configurações["Jump"] = falso
		Alternado = falso
		TweenService:Create(UIElements["Círculo"], TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), { Posição = UDim2.new(0.22, 0, 0.5, 0) }):Play()
		TweenService:Criar(UIElements["Slide"], TweenInfo.new(0.3), { Cor de fundo3 = Cor3.fromRGB(20, 20, 20) }):Reproduzir()
	outro
		Configurações["Jump"] = verdadeiro
		Alternado = verdadeiro
		TweenService:Create(UIElements["Círculo"], TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), { Posição = UDim2.new(0.772, 0, 0.5, 0) }):Play()
		TweenService:Criar(UIElements["Slide"], TweenInfo.new(0.3), { Cor de fundo3 = Cor3.fromRGB(37, 150, 255) }):Reproduzir()
	fim
fim)

UIElements["Reproduzir"].MouseButton1Up:Conectar(função()
	se não for Settings.Config["Start"] ou não Settings.Config["End"] então retorne end
	se não Configurações["Iniciado"] então
		Configurações["Iniciado"] = verdadeiro
		IniciarThread()
	outro
		Configurações["Iniciado"] = falso
		Fim do tópico (falso)
	fim
fim)

se Notificação então
	Notificação:SetupJJs(Options.Experiments)
fim

Solicitação:Post('https://scripts-zvyz.glitch.me/api/count')
