# virtual-interactive-pet-app-using-python-and-streamlit
# Learn how to: Use st.session_state to manage app memory Add interactivity with buttons like Feed, Play, Sleep, and Talk Display emojis and pet moods dynamically Restart the game and reset all stats


import streamlit as st

# --- SIDEBAR: Choose Pet Type and Name ---
st.sidebar.markdown("## 🐾 Pet Setup")

# Pet type selector
pet_type = st.sidebar.selectbox("Choose your pet type:", ["Dog", "Cat", "Fox"])

# Set emoji based on type
if pet_type == "Dog":
    pet_emoji = "🐶"
elif pet_type == "Cat":
    pet_emoji = "🐱"
else:
    pet_emoji = "🦊"

# Pet name input
pet_name = st.sidebar.text_input("Give your pet a name:", value="Snowy")

# Save the name to session state
if 'last_pet_name' not in st.session_state or st.session_state.last_pet_name != pet_name:
    st.session_state.hunger = 5
    st.session_state.energy = 5
    st.session_state.happiness = 5
    st.session_state.last_pet_name = pet_name

# Initialize session state if not already done
if 'hunger' not in st.session_state:
    st.session_state.hunger = 5
    st.session_state.energy = 5
    st.session_state.happiness = 5
    st.session_state.pet_name = "Snowy 🐾"

# --- Title and layout ---
st.title("🐾 Virtual Pet")
st.subheader(f"Say hello to {pet_name} the {pet_type.lower()} {pet_emoji}!")

# --- Mood logic ---
def get_mood():
    if st.session_state.hunger >= 8:
        return "🍽️ Hungry"
    elif st.session_state.energy <= 3:
        return "😴 Tired"
    elif st.session_state.happiness >= 7:
        return "😊 Happy"
    else:
        return "😐 Okay"

# --- Display pet mood as text ---
def show_pet_image(mood):
    st.markdown(f"### 🖼️ {pet_name} feels {mood.lower()} {pet_emoji}")

# --- Actions ---
def feed():
    st.session_state.hunger = max(st.session_state.hunger - 2, 0)
    st.session_state.happiness += 1
    st.success("🍖 Yum! Your pet enjoyed the meal.")

def play():
    st.session_state.happiness += 2
    st.session_state.energy = max(st.session_state.energy - 2, 0)
    st.session_state.hunger += 1
    st.success("🎾 That was fun!")

def sleep():
    st.session_state.energy += 3
    st.session_state.hunger += 1
    st.success("😴 Your pet had a nice nap.")

def talk():
    st.info(f"🗣️ {pet_name} says: 'I feel {get_mood()}'")

# --- Show status ---
mood = get_mood()
st.markdown("### 🧾 Pet's Current Status")
st.write(f"**Hunger:** {st.session_state.hunger} 🍗")
st.write(f"**Energy:** {st.session_state.energy} ⚡")
st.write(f"**Happiness:** {st.session_state.happiness} 🎈")
st.write(f"**Mood:** {mood}")

# --- Show mood text ---
show_pet_image(mood)

# --- Buttons for interaction ---
st.markdown("### 💬 What would you like to do?")
col1, col2, col3, col4 = st.columns(4)

with col1:
    if st.button("🍽️ Feed"):
        feed()
with col2:
    if st.button("🎮 Play"):
        play()
with col3:
    if st.button("🛌 Sleep"):
        sleep()
with col4:
    if st.button("🗣️ Talk"):
        talk()

# --- Reset Button ---
if st.button("🔁 Restart Pet"):
    for key in list(st.session_state.keys()):
        del st.session_state[key]
    st.rerun()
