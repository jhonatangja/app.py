import streamlit as st
import pandas as pd
from datetime import date

# --- CONFIGURAÇÃO DA PÁGINA ---
st.set_page_config(page_title="Diário de Bordo", page_icon="🚛")

# --- DADOS DAS ROTAS (SIMULANDO SUA PLANILHA) ---
# Aqui eu coloquei algumas das rotas que vi nos seus arquivos.
# No sistema final, isso viria de um arquivo ou do Google Sheets.
db_rotas = {
    'RVD': [
        {'id': 1, 'desc': 'Montividiu, Sto Antonio, Acreuna...', 'km': 399, 'valor': 478.8},
        {'id': 2, 'desc': 'Montidividiu, Santa H...', 'km': 299, 'valor': 358.8},
        {'id': 3, 'desc': 'S. Antonio, Santa Helena...', 'km': 178, 'valor': 213.6},
    ],
    'JTI': [
        {'id': 1, 'desc': 'Jataí, Rio Verde, Jataí', 'km': 184, 'valor': 239.2},
        {'id': 2, 'desc': 'Jataí, Mineiros, Jataí', 'km': 216, 'valor': 259.2},
    ],
    'QUI': [
        {'id': 1, 'desc': 'Quirinópolis, Rio Verde...', 'km': 221, 'valor': 221.0},
    ]
}

# --- CABEÇALHO ---
st.title("🚛 Controle de Rotas")
st.write("Lançamento rápido pelo celular")

# --- FORMULÁRIO DE LANÇAMENTO ---
with st.form("lancamento_form"):
    col1, col2 = st.columns(2)
    
    with col1:
        data = st.date_input("Data da Viagem", date.today())
    with col2:
        filial = st.selectbox("Filial", ["RVD", "JTI", "QUI"])
    
    # Seleção da Rota baseada na Filial
    rotas_da_filial = db_rotas[filial]
    # Cria uma lista bonita para o menu: "1 - Jataí, Rio Verde..."
    opcoes_rotas = [f"{r['id']} - {r['desc'][:20]}..." for r in rotas_da_filial]
    rota_selecionada = st.selectbox("Selecione a Rota", opcoes_rotas)
    
    # Configurações de Custo (Isso pode ficar fixo ou editável)
    st.subheader("💰 Custos e Valores")
    c1, c2 = st.columns(2)
    with c1:
        preco_diesel = st.number_input("Preço Diesel (R$)", value=6.10, step=0.10)
    with c2:
        media_km_l = st.number_input("Média Km/L", value=3.5, step=0.1)

    # --- CÁLCULOS AUTOMÁTICOS ---
    # Pega o ID da rota selecionada (o número antes do traço)
    id_rota = int(rota_selecionada.split(' - ')[0])
    
    # Busca os dados completos da rota
    dados_rota = next(item for item in rotas_da_filial if item["id"] == id_rota)
    
    km_total = dados_rota['km']
    valor_frete = dados_rota['valor']
    
    # Cálculo do Gasto
    litros_necessarios = km_total / media_km_l
    custo_combustivel = litros_necessarios * preco_diesel
    lucro = valor_frete - custo_combustivel

    # --- MOSTRAR RESUMO ANTES DE SALVAR ---
    st.info(f"""
    📍 **Rota:** {dados_rota['desc']}
    🛣️ **KM:** {km_total} km
    ⛽ **Gasto Est. Diesel:** R$ {custo_combustivel:.2f}
    💵 **A Receber:** R$ {valor_frete:.2f}
    📈 **Lucro:** R$ {lucro:.2f}
    """)

    # Botão de Salvar
    submitted = st.form_submit_button("💾 SALVAR LANÇAMENTO")

    if submitted:
        # AQUI ENTRARIA O CÓDIGO PARA SALVAR NO GOOGLE SHEETS
        # Por enquanto, vamos apenas mostrar na tela que deu certo
        
        novo_lancamento = {
            "Data": data,
            "Filial": filial,
            "Rota": id_rota,
            "KM": km_total,
            "Valor": valor_frete,
            "Gasto Diesel": round(custo_combustivel, 2)
        }
        st.success("✅ Rota registrada com sucesso!")
        st.json(novo_lancamento) # Mostra os dados salvos
