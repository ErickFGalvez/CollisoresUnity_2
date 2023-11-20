# Nomes
Erick Felipe Mendes Galvez

# Atividade Collisores 2.0
Atividade passada pela orientadora Aline Firmino, Desenvolver uma cena aplicado cada um dos tipos de colisores 3D.
Devem ser utilizados todos os tipos de colisões e Triggers,depois acrescentar as seguintes coisas:
Criar um Menu
no jogo principal incluir um HUD dinâmico
Utilizar uma colisão para finalizar o jogo e ir para uma cena de créditos/fim de jogo
Nos créditos inserir a opção de fechar o jogo, reiniciar ou voltar para o menu inicial
O ReadMe deve ser atualizado com as informações e explicação das alterações

## Requisitos
Para ver essa cena é preciso o Unity com a versão 2021.3.26f1

Instalação

1.clonar o projeto https://drive.google.com/file/d/1PncEsJ1cTp_lDpdxITo7VTfhdEk2Y7zf/view?usp=sharing

2.Abrir o projeto no Unity.

Desenvolvimento

Para criar esse projeto foram utilizados os seguintes passos:

Criar 4 game objects 1 espera,2 cubo e uma superficie plana.

![Captura de tela 2023-10-30 163709](https://github.com/ErickFGalvez/CollisoresUnity/assets/128325280/1eb241e7-58b7-4394-bd0c-e97702eb2374)

3. Criar 2 Scripts CollisionEvents e MovimentoCubo.

![Captura de tela 2023-10-30 163730](https://github.com/ErickFGalvez/CollisoresUnity/assets/128325280/6f20457a-f9da-4b8d-b003-7e02b271758d)

## segue o codigo comentado e uma explicaçao rapida do CollisionEvents.

O CollisionEvents é um script Unity que fornece funcionalidade para lidar com eventos de colisão e acionar ações com base nessas colisões. Ele também é responsável por alterar a cor do objeto no qual está anexado e ativar um Canvas de Fim de Cena quando uma determinada condição é atendida.

Como Usar
Anexando o Script:

Anexe o script CollisionEvents.cs a qualquer objeto que deseja monitorar colisões. Certifique-se de que o objeto tenha um Collider (por exemplo, um Collider de caixa) para ativar eventos de colisão.

Configurações no Inspector:

No Inspector do objeto ao qual você anexou o script, você pode configurar as seguintes variáveis públicas:

FimDaSenaCanvas (Fim da Cena Canvas): Arraste o GameObject que representa o Canvas que você deseja ativar quando a condição for atendida.

Texto: Arraste o componente Text que você deseja ativar juntamente com o Canvas no Fim da Cena.

Personalização:

Você pode personalizar as condições de colisão e as ações realizadas em resposta a essas colisões. O script atual inclui os seguintes métodos:

OnCollisionEnter: Chamado quando uma colisão começa.
OnCollisionExit: Chamado quando uma colisão termina.
OnCollisionStay: Chamado enquanto uma colisão está em andamento.
OnTriggerEnter: Chamado quando um objeto entra em um Trigger Collider.

## codigo

using UnityEngine;

public class CollisionEvents : MonoBehaviour
{
    public GameObject FimDaSenaCanvas; // Referência a um Canvas que será ativado no final
    public Text texto; // Referência a um componente Text que será ativado no final

    void Start(){
        // Desativa o Canvas no início do jogo
        FimDaSenaCanvas.SetActive(false);
        
        // Desativa o componente Text no início do jogo
        texto.enabled = false;
    }

    void OnCollisionEnter(Collision collision)
    {
        // Este método é chamado quando uma colisão começa
        if (collision.gameObject.name == "Cube")
        {
            Debug.Log("Enter"); // Registra uma mensagem no console

            // Muda a cor do objeto atual para vermelho
            GetComponent<Renderer>().material.color = Color.red;
        }
    }

    void OnCollisionExit(Collision collision)
    {
        // Este método é chamado quando uma colisão termina
        if (collision.gameObject.name == "Cube")
        {
            Debug.Log("Exit"); // Registra uma mensagem no console

            // Muda a cor do objeto atual para verde
            GetComponent<Renderer>().material.color = Color.green;
        }
    }

    public void FimDaCena()
    {
        // Este método pode ser chamado de fora do script
        if (FimDaSenaCanvas != null)
        {
            // Ativa o Canvas de Fim da Cena
            FimDaSenaCanvas.SetActive(true);
            
            // Ativa o componente Text
            texto.enabled = true; 
        }
    }

    void OnCollisionStay(Collision collision)
    {
        // Este método é chamado enquanto uma colisão está em andamento
        if (collision.gameObject.name == "Cube")
        {
            Debug.Log("Stay"); // Registra uma mensagem no console

            // Faz o objeto atual girar no eixo Z com uma taxa específica
            collision.transform.localEulerAngles += new Vector3(0, 0, -10) * Time.deltaTime;
        }
    }

    void OnTriggerEnter(Collider other)
    {
        // Este método é chamado quando um objeto entra em um Trigger Collider
        if (other.gameObject.CompareTag("Finish"))
        {
            // Chama o método FimDaCena para ativar o Canvas e o Text
            FimDaCena();
        }
    }
}


## segue o codigo comentado e uma explicaçao rapida do MovimentoCubo.

O MovimentoCubo é um script Unity que controla o movimento vertical de um cubo. Ele faz o cubo se mover para cima e para baixo de forma oscilante com base no tempo.

Como Usar
Anexando o Script:

Anexe o script MovimentoCubo.cs a um objeto no Unity. Esse objeto será o cubo que você deseja mover verticalmente.

Configurações no Inspector:

No Inspector do objeto ao qual você anexou o script, você pode configurar as seguintes variáveis públicas:

Velocidade: Defina a velocidade com a qual o cubo se moverá para cima e para baixo.

Distância Máxima: Especifique a distância máxima que o cubo percorrerá em sua oscilação vertical.

Posição Inicial:

O script armazena a posição inicial do cubo no momento do início do jogo. Isso é feito no método Start(). Certifique-se de que o cubo está na posição desejada quando você inicia a cena.

Movimento Vertical Oscilante:

O método Update() é chamado a cada quadro do jogo. Nele, o script calcula um deslocamento vertical baseado no tempo e atualiza a posição do cubo em relação à posição inicial. Isso cria um movimento vertical oscilante.

## codigo

** public class MovimentoCubo : MonoBehaviour
{
    public float velocidade = 2.0f; // A velocidade de movimento do cubo
    public float distanciaMaxima = 2.0f; // A distância máxima que o cubo vai percorrer para cima e para baixo
    private Vector3 posicaoInicial; // A posição inicial do cubo

    void Start()
    {
        // Armazena a posição inicial do cubo
        posicaoInicial = transform.position;
    }

    void Update()
    {
        // Calcula o deslocamento vertical com base no tempo
        float deslocamentoVertical = Mathf.Sin(Time.time * velocidade) * distanciaMaxima;

        // Atualiza a posição do cubo
        transform.position = posicaoInicial + new Vector3(0, deslocamentoVertical, 0);
    }
}


## Atualiçoes feitas

para essa atualizacao foram criadas 2 scrips para serem anexados em 2 canvas, tambem foi adicionado 1 timer para o hud.

![Captura de tela 2023-11-20 190147](https://github.com/ErickFGalvez/CollisoresUnity_2/assets/128325280/3729c449-d3b7-4ada-a0a0-6676d545129d)

foi adicionado esse script nesse canvas

![Captura de tela 2023-11-20 190219](https://github.com/ErickFGalvez/CollisoresUnity_2/assets/128325280/90c2a36d-0065-4ea4-bbc3-db25fad660f3)

segue a explicaçao e codigo comentado

## README
Menu Principal Unity
Este é um script simples para um menu principal em um jogo Unity. Ele inclui funcionalidades básicas de iniciar o jogo e sair.

Instruções de Uso:
Anexando ao GameObject:

Anexe este script a um objeto no seu cena Unity que servirá como o menu principal.
Configurações no Editor:

No Editor Unity, você verá uma nova seção no script chamada "Nome do Level de Jogo". Aqui, insira o nome da cena que você deseja carregar quando o jogador clicar em "Jogar".
Funções do Menu:

Jogar(): Carrega a cena especificada no campo nomeDoLevelDeJogo usando o SceneManager.LoadScene.
Sair(): Exibe uma mensagem de log indicando que o jogo está saindo e chama Application.Quit() para encerrar a aplicação (somente funciona em builds standalone).


public class MenuPrincipal : MonoBehaviour
{
    // Nome da cena que será carregada ao clicar em "Jogar"
    [SerializeField] private string nomeDoLevelDeJogo;

    // Função chamada quando o botão "Jogar" é pressionado
    public void Jogar()
    {
        // Carrega a cena especificada usando o SceneManager
        SceneManager.LoadScene(nomeDoLevelDeJogo);
    }

    // Função chamada quando o botão "Sair" é pressionado
    public void Sair()
    {
        // Exibe uma mensagem de log indicando que o jogo está saindo
        Debug.Log("Sair do Jogo");
        
        // Encerra a aplicação (funciona apenas em builds standalone)
        Application.Quit();
    }
}

![Captura de tela 2023-11-20 191102](https://github.com/ErickFGalvez/CollisoresUnity_2/assets/128325280/d113ea2a-0001-49c2-97d4-8776a23ff72d)

Codigos adicionados nesse canvas

![Captura de tela 2023-11-20 191209](https://github.com/ErickFGalvez/CollisoresUnity_2/assets/128325280/8236be69-d773-4a5c-bf06-31662e5277ec)

segue o codigo de reiniciar explicado e comentado

## README

Botão de Retorno ao Menu Unity
Este script Unity implementa um botão simples que, quando pressionado, reinicia o jogo carregando a cena especificada no campo "Nome do Level de Jogo".

Instruções de Uso:
Anexando ao GameObject:

Anexe este script a um objeto na sua cena Unity que servirá como um botão para retornar ao menu.
Configurações no Editor:

No Editor Unity, você verá uma nova seção no script chamada "Nome do Level de Jogo". Aqui, insira o nome da cena que você deseja carregar quando o jogador clicar em "Reiniciar".
Função do Botão:

Reiniciar(): Carrega a cena especificada no campo nomeDoLevelDeJogo usando o SceneManager.LoadScene.

using UnityEngine;
using UnityEngine.SceneManagement;

public class BackToMenuButton : MonoBehaviour
{
    // Nome da cena que será carregada ao clicar em "Reiniciar"
    [SerializeField] private string nomeDoLevelDeJogo;

    // Função chamada quando o botão "Reiniciar" é pressionado
    public void Reiniciar()
    {
        // Carrega a cena especificada usando o SceneManager
        SceneManager.LoadScene(nomeDoLevelDeJogo);
    }
}

segue codigo do timer explicado e comentado

## README

Contador de Tempo Unity
Este script Unity implementa um contador de tempo simples, exibindo minutos e segundos em um componente de texto (TMP_Text).

Instruções de Uso:
Anexando ao GameObject:

Anexe este script a um objeto na sua cena Unity que servirá como o contador de tempo.
Configurações no Editor:

No Editor Unity, configure o campo "Txt Time" arrastando e soltando um componente TMP_Text que exibirá o tempo.
Defina o tempo inicial no campo "Time Value".
Funcionalidades:

O contador de tempo aumenta a cada segundo e exibe o tempo formatado em minutos e segundos no componente de texto.

using UnityEngine;
using TMPro;

public class Timer : MonoBehaviour
{
    // Componente de texto para exibir o tempo
    [SerializeField] private TMP_Text txttime;
    
    // Valor inicial do tempo
    [SerializeField] private float timeValue;

    // Método chamado quando o script é inicializado
    void Start()
    {
        // Invoca repetidamente o método "IncreaseTime" a cada segundo
        InvokeRepeating("IncreaseTime", 1f, 1f);
    }

    // Método para aumentar o tempo
    private void IncreaseTime()
    {
        // Se o tempo for menor que zero, não faz nada
        if (timeValue < 0f) return;

        // Incrementa o valor do tempo
        timeValue++;

        // Exibe o tempo atualizado
        DisplayTime(timeValue);
    }

    // Método para exibir o tempo formatado no componente de texto
    private void DisplayTime(float timeToDisplay)
    {
        // Garante que o tempo exibido nunca seja negativo
        if (timeToDisplay < 0f)
        {
            timeToDisplay = 0f;
        }

        // Calcula minutos e segundos a partir do tempo total
        float minutes = Mathf.FloorToInt(timeToDisplay / 60);
        float seconds = Mathf.FloorToInt(timeToDisplay % 60);

        // Formata o tempo como minutos:segundos e exibe no componente de texto
        txttime.text = string.Format("{0:00}:{1:00}", minutes, seconds);
    }
}

# Segue link do video do codigo funcionando

https://youtu.be/QfnuaBLnnUY
