# 채팅 앱

이 프로젝트는 Streamlit을 사용하여 Gemini AI 모델과 상호작용할 수 있는 간단한 채팅 앱을 구현합니다. 사용자가 질문을 입력하면, Gemini AI가 응답을 제공합니다.

## 필요 조건

- Python 3.7 이상
- Streamlit
- Google AI Client Library

## 설치

1. 이 저장소를 클론합니다.
   ```bash
   git clone https://github.com/yourusername/chat-app.git
   cd chat-app

2. 필요한 라이브러리를 설치합니다.

    pip install streamlit google-genai 

3. secrets.toml 파일을 생성하여 Gemini API 키를 추가합니다.

    [GEMINI_API_KEY]
        value = "YOUR_API_KEY"

## 사용 방법

1. 앱을 실행합니다.

    streamlit run app.py
2. 웹 브라우저에서 http://localhost:8501에 접속합니다.

3. 질문을 입력하고 Gemini AI의 응답을 기다립니다.

## 코드 예시

아래는 앱의 주요 코드 입니다

import streamlit as st
from google import genai

def main():
    st.title("채팅 앱")

    # Gemini 클라이언트 초기화
    client = genai.Client(api_key=st.secrets["GEMINI_API_KEY"])

    # 세션 상태에서 채팅 기록 초기화
    if "messages" not in st.session_state:
        st.session_state.messages = []

    # 저장된 채팅 기록 화면에 표시
    for message in st.session_state.messages:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])

    # 사용자 입력 필드
    user_input = st.chat_input("질문을 입력하세요...")

    # 사용자가 입력했을 때 실행
    if user_input:
        # 사용자 메시지를 채팅 기록에 추가
        st.session_state.messages.append({"role": "user", "content": user_input})

        # 사용자 메시지를 화면에 표시
        with st.chat_message("user"):
            st.markdown(user_input)

        # Gemini API에서 응답 받기
        with st.chat_message("assistant"):
            with st.spinner("생각 중..."):
                response = client.models.generate_content(
                    model="gemini-2.5-flash",
                    contents=["어르신들 대상의 서비스니까 친절하고 살갑게 대답해줘, 그리고 대답은 알기 쉬운 용어를 써서 말해줘, 유저 질문이 노인 복지 관련 내용이 아니면 노인 복지 내용만 질문 하도록 해줘" + user_input]
                )
                assistant_message = response.text
                st.markdown(assistant_message)

        # AI 응답을 채팅 기록에 추가
        st.session_state.messages.append({"role": "assistant", "content": assistant_message})

if __name__ == "__main__":
    main()


## 기여

기여를 원하시면 이 저장소를 포크하고, 변경 사항을 푸시한 후 Pull Request를 제출해주세요.

