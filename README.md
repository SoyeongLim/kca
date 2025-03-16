import streamlit as st
import json
import os

# 파일 경로 설정
DATA_FILE = "events_data.json"

# 행사 정보를 저장할 리스트 (기존 데이터가 있으면 불러오기)
def load_events():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, 'r', encoding='utf-8') as file:
            return json.load(file)
    return []

# 데이터를 파일에 저장하는 함수
def save_events(events):
    with open(DATA_FILE, 'w', encoding='utf-8') as file:
        json.dump(events, file, ensure_ascii=False, indent=4)

# 이벤트 데이터 추가
def add_event(events):
    name = st.text_input("행사 이름")
    income = st.number_input("수입", min_value=0)
    participants = st.number_input("참여 인원", min_value=0)
    expenses = st.text_area("지출 항목 (쉼표로 구분)")

    if st.button("행사 저장"):
        event_data = {
            "name": name,
            "income": income,
            "participants": participants,
            "expenses": expenses.split(",") if expenses else []
        }
        events.append(event_data)
        save_events(events)
        st.success(f"'{name}' 저장 완료!")

# 행사 목록 출력
def display_events(events):
    st.header("📋 행사 목록")
    for event in events:
        st.write(f"**행사:** {event['name']}")
        st.write(f"수입: {event['income']} 원")
        st.write(f"참여 인원: {event['participants']} 명")
        st.write(f"지출: {', '.join(event['expenses'])}")
        st.write("---")

# 앱 실행
def main():
    events = load_events()  # 저장된 행사 데이터 불러오기
    
    # 본인만 데이터 추가 가능
    if "user" not in st.session_state:
        st.session_state["user"] = lsy

    if st.session_state["user"] is lsy:
        st.session_state["user"] = st.text_input("이메일을 입력하세요 (관리자용):")
        
        if st.session_state["user"] == "dlathdud1212":  # 관리자 이메일 확인
            st.success("관리자 로그인 성공!")
        else:
            st.error("관리자만 접근 가능합니다.")
    else:
        add_event(events)  # 행사 추가 기능
        display_events(events)  # 행사 목록 조회

if __name__ == "__main__":
    main()
