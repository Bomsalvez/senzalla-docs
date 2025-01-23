# Estrutura de Pastas e Arquivos para Projetos Angular

Este documento descreve uma estrutura de pastas e arquivos recomendada para projetos Angular modernos. A organização visa facilitar a manutenção, escalabilidade e entendimento do código.

---

## Estrutura Geral

```
src/
├── config/                   # Configurações globais e infraestrutura
│   ├── app.routes.ts         # Configuração de rotas globais
│   ├── app.providers.ts      # Configuração de providers globais
│   └── index.ts              # Exportação centralizada (opcional)
├── models/                   # Modelos e interfaces compartilhadas
│   ├── user.model.ts
│   ├── product.model.ts
│   └── index.ts
├── pages/                    # Páginas direcionáveis (standalone components com URLs)
│   ├── feature-one/          
│   │   ├── components/       
│   │   ├── services/         
│   │   │   ├── feature.service.ts # Serviço principal com tratativas
│   │   │   └── requests/     # Pasta para requisições HTTP específicas
│   │   │       ├── feature-api.service.ts
│   │   │       └── ...       # Outros arquivos de requisições
│   │   ├── feature-one.component.ts
│   │   └── feature-one.routes.ts
│   ├── feature-two/          
│   │   ├── components/       
│   │   ├── services/         
│   │   │   ├── feature.service.ts
│   │   │   └── requests/     
│   │   │       ├── feature-api.service.ts
│   │   │       └── ...
│   │   ├── feature-two.component.ts
│   │   └── feature-two.routes.ts
│   └── ...
├── app/                      # Arquivos centrais do app
│   ├── app.component.ts      # Componente raiz
│   ├── app.component.html    # Template raiz (estrutura básica do app)
│   ├── main.ts               # Arquivo principal
│   └── bootstrap.ts          # Bootstrap da aplicação
├── core/                     # Serviços e funcionalidades globais
│   ├── interceptors/         # Interceptadores HTTP
│   ├── guards/               # Guards de rota
│   ├── services/             # Serviços globais
│   └── index.ts              # Exportação dos providers globais
├── shared/                   # Componentes, pipes e diretivas reutilizáveis
│   ├── components/           # Componentes compartilhados
│   │   ├── header/           # Cabeçalho compartilhado
│   │   │   ├── header.component.ts
│   │   │   ├── header.component.html
│   │   │   └── header.component.scss
│   │   ├── footer/           # Rodapé compartilhado
│   │   │   ├── footer.component.ts
│   │   │   ├── footer.component.html
│   │   │   └── footer.component.scss
│   │   └── ...               # Outros componentes reutilizáveis
│   ├── directives/           # Diretivas reutilizáveis
│   ├── pipes/                # Pipes reutilizáveis
│   └── shared.styles.scss    # Estilos compartilhados
├── assets/                   # Recursos estáticos (imagens, fontes, etc.)
├── environments/             # Configurações para diferentes ambientes
│   ├── environment.ts
│   └── environment.prod.ts
├── styles/                   # Arquivos de estilo global (CSS, SCSS, etc.)
│   ├── _variables.scss
│   ├── _mixins.scss
│   └── styles.scss
├── index.html                # HTML principal
└── main.ts                   # Arquivo de inicialização
```

---

## Descrição das Pastas

### 1. `config/`
- Armazena configurações globais, como rotas e providers.
- Arquivos típicos:
    - `app.routes.ts`: Configuração de rotas globais.
    - `app.providers.ts`: Configuração de providers globais.
    - `index.ts` (opcional): Exportação centralizada.

### 2. `models/`
- Contém modelos e interfaces que representam os dados usados na aplicação.
- Uso principal: definir contratos para comunicação entre serviços, APIs e componentes.

### 3. `pages/`
- Contém os **standalone components** direcionáveis (com URLs associadas).
- Cada página possui sua própria estrutura interna:
    - **`services/`**: Contém a lógica principal da funcionalidade, como tratativas de dados.
    - **`requests/`**: Dentro de `services/`, é dedicada às requisições HTTP específicas da funcionalidade.

### 4. `app/`
- Contém os arquivos centrais da aplicação:
    - `app.component.ts`: Componente raiz.
    - `bootstrap.ts`: Inicialização da aplicação.

### 5. `core/`
- Centraliza funcionalidades globais, como:
    - Interceptadores HTTP.
    - Guards de rota.
    - Serviços que não são específicos de um componente ou página.

### 6. `shared/`
- Contém elementos reutilizáveis, como:
    - **Componentes compartilhados** (ex.: cabeçalho, rodapé).
    - **Pipes e diretivas reutilizáveis**.

### 7. `assets/`
- Armazena recursos estáticos, como imagens, fontes e ícones.

### 8. `environments/`
- Configurações específicas para diferentes ambientes (ex.: desenvolvimento, produção).

### 9. `styles/`
- Armazena estilos globais e variáveis SCSS compartilhadas.

---

## Benefícios

1. **Organização clara**:
    - Facilita encontrar arquivos e entender a hierarquia do projeto.
2. **Reutilização**:
    - Elementos compartilhados ficam centralizados em `shared/`.
3. **Escalabilidade**:
    - Permite o crescimento do projeto sem perda de organização.
4. **Separação de responsabilidades**:
    - Cada pasta tem uma função clara.

---

## Exemplos de Arquivos

### Exemplo: `feature-api.service.ts` (em `requests/`)
```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class FeatureApiService {
  private readonly apiUrl = 'https://api.example.com/feature';

  constructor(private http: HttpClient) {}

  getData(): Observable<any> {
    return this.http.get(`${this.apiUrl}/data`);
  }

  postData(payload: any): Observable<any> {
    return this.http.post(`${this.apiUrl}/data`, payload);
  }
}
```

### Exemplo: `feature.service.ts` (em `services/`)
```typescript
import { Injectable } from '@angular/core';
import { FeatureApiService } from './requests/feature-api.service';
import { map } from 'rxjs/operators';

@Injectable({ providedIn: 'root' })
export class FeatureService {
  constructor(private featureApi: FeatureApiService) {}

  getProcessedData() {
    return this.featureApi.getData().pipe(
      map(data => {
        // Processamento adicional dos dados
        return data.map(item => ({ ...item, processed: true }));
      })
    );
  }

  saveData(payload: any) {
    // Tratativa antes de enviar os dados
    const processedPayload = { ...payload, timestamp: new Date() };
    return this.featureApi.postData(processedPayload);
  }
}
```

