# Nomes
Erick Felipe Mendes Galvez

# Atividade Collisores 2.0
Atividade passada pela orientadora Aline Firmino, Desenvolver uma cena aplicado cada um dos tipos de colisores 3D.
Devem ser utilizados todos os tipos de colisões e Triggers,depois acrescentar as

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
