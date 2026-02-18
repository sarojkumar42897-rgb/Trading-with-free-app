import streamlit as st
import pandas as pd
import plotly.graph_objects as go
import time
import random

# पेज की बनावट
st.set_page_config(page_title="ट्रेडिंग विद फ्री ऐप", layout="wide", initial_sidebar_state="collapsed")

# ऐप का स्टाइल (CSS)
st.markdown("""
    <style>
    .main { background-color: #0e1117; color: white; }
    .stButton>button { width: 100%; border-radius: 10px; height: 3em; font-weight: bold; }
    </style>
    """, unsafe_allow_html=True)

# डेटा स्टोर करना
if 'balance' not in st.session_state: st.session_state.balance = 0
if 'step' not in st.session_state: st.session_state.step = "level_0"

# --- लेवल 0: फ्री ट्रेनिंग ---
if st.session_state.step == "level_0":
    st.title("🎯 लेवल 0: ट्रेडिंग की पहली परीक्षा")
    st.subheader("क्या आप बता सकते हैं मार्केट का अगला कदम?")
    
    # डमी चार्ट डेटा
    df = pd.DataFrame({
        'Step': [1, 2, 3, 4, 5, 6, 7],
        'Price': [100, 105, 102, 108, 110, 107, 112]
    })
    fig = go.Figure(data=[go.Scatter(x=df['Step'], y=df['Price'], line=dict(color='#00ff00', width=4))])
    fig.update_layout(template="plotly_dark", height=300)
    st.plotly_chart(fig, use_container_width=True)

    st.info("संकेत: चार्ट को देखें, लोअर पॉइंट पिछले पॉइंट से ऊपर है (Higher Lows)।")

    c1, c2 = st.columns(2)
    with c1:
        if st.button("🚀 ऊपर जाएगा (BUY)"):
            st.success("अद्भुत पहचान! आप में एक प्रो-ट्रेडर छिपा है।")
            time.sleep(1.5)
            st.session_state.step = "jackpot"
            st.rerun()
    with c2:
        if st.button("📉 नीचे गिरेगा (SELL)"):
            st.error("ओह! ट्रेंड ऊपर का था। फिर से कोशिश करें।")

# --- जैकपॉट स्क्रीन ---
elif st.session_state.step == "jackpot":
    st.balloons()
    st.title("🎊 बधाई हो! 🎊")
    st.header("आपने अपनी काबिलियत साबित कर दी।")
    st.success("इनाम के तौर पर आपको मिलते हैं:")
    st.markdown("<h1 style='text-align: center; color: #FFD700;'>₹10,00,000</h1>", unsafe_allow_html=True)
    st.write("यह वर्चुअल कैश आपके सीखने के लिए है।")
    
    if st.button("ट्रेडिंग डैशबोर्ड में प्रवेश करें"):
        st.session_state.balance = 1000000
        st.session_state.step = "dashboard"
        st.rerun()

# --- मुख्य डैशबोर्ड ---
elif st.session_state.step == "dashboard":
    st.title("📊 ट्रेडिंग विद फ्री ऐप - डैशबोर्ड")
    st.sidebar.metric("आपका बैलेंस", f"₹{st.session_state.balance:,}")
    
    col1, col2, col3 = st.columns(3)
    col1.metric("कुल प्रॉफिट", "₹0", "0%")
    col2.metric("एक्टिव ट्रेड्स", "0")
    col3.metric("रैंक", "Rookie")

    st.write("---")
    st.subheader("लाइव सिम्युलेटर")
    
    # रैंडम प्राइस मूवमेंट
    price = random.randint(200, 300)
    st.header(f"स्टॉक भाव: ₹{price}")
    
    c1, c2 = st.columns(2)
    c1.button("BUY (खरीदें)")
    c2.button("SELL (बेचें)")
    
    if st.button("गेम रीसेट करें"):
        st.session_state.step = "level_0"
        st.rerun()
