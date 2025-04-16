// Importação de ícones do pacote phosphor-icons para usar nos destaques da seção Hero
import { Coffee, Package, ShoppingCart, Timer } from '@phosphor-icons/react'
import { useTheme } from 'styled-components'



// Hook de tema vindo do styled-components para acessar cores e estilos globais\import { useTheme } from 'styled-components'

// Componente que renderiza cada card individual de café
import { CoffeeCard } from '../../components/CoffeeCard'

// Importando o Axios para fazer a requisição para a API
import axios from 'axios'

// Importação de componentes estilizados da pasta de estilos
import { CoffeeList, Heading, Hero, HeroContent, Info } from './styles'

// Importando hooks do React
import { useEffect, useState } from 'react'

// Interface que define a estrutura dos objetos de café
interface Coffee {
  id: string;
  title: string;
  description: string;
  tags: string[];
  price: number;
  image: string;
  quantity: number;
};

// Componente principal da página Home
export function Home() {
  const theme = useTheme(); // Hook para acessar o tema definido no styled-components

  // Estado que armazena os cafés buscados da API
  const [coffees, setCoffees] = useState<Coffee[]>([]);

  // Estado para controlar se a página está carregando
  const [loading, setLoading] = useState(true);

  // useEffect é chamado assim que o componente é montado
  useEffect(() => {
    // Fazendo requisição GET para buscar os cafés
    axios.get('http://localhost:3000/coffees')
      .then((res) => {
        setCoffees(res.data); // Salvando os cafés no estado
      })
      .catch((err) => {
        console.error('Erro ao carregar cafés:', err); // Tratamento de erro
      })
      .finally(() => {
        setLoading(false); // Finaliza o carregamento
      });
  }, []);

  // Se ainda estiver carregando, mostra a mensagem de loading
  if (loading) {
    return (
      <div style={{ textAlign: 'center', marginTop: '2rem' }}>
        <strong>Aguarde, estamos preparando o melhor café pra você!</strong>
      </div>
    )
  }

  // Função para incrementar a quantidade do café
  function incrementQuantity(id: string) {
    setCoffees((prevCoffees) =>
      prevCoffees.map((coffee) => {
        if (coffee.id === id && coffee.quantity < 5) {
          return { ...coffee, quantity: coffee.quantity + 1 }
        }
        return coffee;
      })
    );
  }

  // Função para decrementar a quantidade do café
  function decrementQuantity(id: string) {
    setCoffees((prevCoffees) =>
      prevCoffees.map((coffee) => {
        if (coffee.id === id && coffee.quantity > 0) {
          return { ...coffee, quantity: coffee.quantity - 1 }
        }
        return coffee;
      })
    );
  }

  // JSX da tela principal
  return (
    <div>
      {/* Seção de destaque (Hero) com ícones e texto promocional */}
      <Hero>
        <HeroContent>
          <div>
            <Heading>
              <h1>Encontre o café perfeito para qualquer hora do dia</h1>
              <span>Com o Coffee Delivery você recebe seu café onde estiver, a qualquer hora</span>
            </Heading>

            <Info>
              <div>
                <ShoppingCart
                  size={32}
                  weight="fill"
                  color={theme.colors.background}
                  style={{ backgroundColor: theme.colors['yellow-dark'] }}
                />
                <span>Compra simples e segura</span>
              </div>

              <div>
                <Package
                  size={32}
                  weight="fill"
                  color={theme.colors.background}
                  style={{ backgroundColor: theme.colors['base-text'] }}
                />
                <span>Embalagem mantém o café intacto</span>
              </div>

              <div>
                <Timer
                  size={32}
                  weight="fill"
                  color={theme.colors.background}
                  style={{ backgroundColor: theme.colors.yellow }}
                />
                <span>Entrega rápida e rastreada</span>
              </div>

              <div>
                <Coffee
                  size={32}
                  weight="fill"
                  color={theme.colors.background}
                  style={{ backgroundColor: theme.colors.purple }}
                />
                <span>O café chega fresquinho até você</span>
              </div>
            </Info>
          </div>

          <img src="/images/hero.svg" alt="Café do Coffee Delivery" />
        </HeroContent>
        <img src="/images/hero-bg.svg" id="hero-bg" alt="" />
      </Hero>

      {/* Lista dos cafés carregados dinamicamente */}
      <CoffeeList>
        <h2>Nossos cafés</h2>
        <div>
          {coffees.map((coffee) => (
            <CoffeeCard
              key={coffee.id}
              coffee={coffee}
              incrementQuantity={incrementQuantity}
              decrementQuantity={decrementQuantity}
            />
          ))}
        </div>
      </CoffeeList>
    </div>
  );
}
