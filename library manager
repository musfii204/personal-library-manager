import json
import streamlit as st

LIBRARY_FILE = "library.json"

def load_library():
    try:
        with open(LIBRARY_FILE, "r") as file:
            return json.load(file)
    except (FileNotFoundError, json.JSONDecodeError):
        return []

def save_library(library):
    with open(LIBRARY_FILE, "w") as file:
        json.dump(library, file, indent=4)

def add_book(library, title, author, year, genre, read_status, rating):
    book = {
        "title": title,
        "author": author,
        "year": int(year),
        "genre": genre,
        "read": read_status,
        "rating": rating
    }
    library.append(book)
    save_library(library)

def remove_book(library, title):
    for book in library:
        if book["title"].lower() == title.lower():
            library.remove(book)
            save_library(library)
            return True
    return False

def search_books(library, query):
    return [book for book in library if query.lower() in book["title"].lower() or query.lower() in book["author"].lower()]

def display_books(library):
    if not library:
        st.warning("🚨 No books in the library.")
        return
    
    for book in library:
        st.markdown(f"""
        <div style='background-color: rgba(255,255,255,0.8); padding: 15px; border-radius: 10px; box-shadow: 2px 2px 10px rgba(0,0,0,0.1); margin-bottom: 10px;'>
            <b>📖 Title:</b> {book['title']}<br>
            <b>✍ Author:</b> {book['author']}<br>
            <b>📅 Year:</b> {book['year']}<br>
            <b>📚 Genre:</b> {book['genre']}<br>
            <b>✅ Read:</b> {'✔' if book['read'] else '✘'}<br>
            <b>⭐ Rating:</b> {book['rating']}/5
        </div>
        """, unsafe_allow_html=True)

def display_statistics(library):
    total_books = len(library)
    read_books = sum(1 for book in library if book["read"])
    percentage_read = (read_books / total_books * 100) if total_books > 0 else 0
    avg_rating = sum(book["rating"] for book in library) / total_books if total_books > 0 else 0
    
    st.metric("📚 Total Books", total_books)
    st.metric("📖 Books Read", f"{read_books} ({percentage_read:.2f}%)")
    st.metric("⭐ Average Rating", f"{avg_rating:.2f}/5")

def main():
    st.markdown("""
    <h1 style='text-align: center;'>📚 Personal Library Manager 📖</h1>
    """, unsafe_allow_html=True)
    
    library = load_library()
    menu = ["➕ Add a Book", "🗑️ Remove a Book", "🔍 Search for a Book", "📂 Display All Books", "📊 Display Statistics"]
    choice = st.sidebar.selectbox("📌 Menu", menu)
    
    if choice == "➕ Add a Book":
        st.subheader("📖 Add a New Book")
        title = st.text_input("📌 Title")
        author = st.text_input("✍ Author")
        year = st.number_input("📅 Publication Year", min_value=0, step=1)
        genre = st.text_input("📚 Genre")
        read_status = st.checkbox("✅ Have you read it?")
        rating = st.slider("⭐ Rate the book", 1, 5, 3)
        if st.button("➕ Add Book", help="Click to add a new book"):
            add_book(library, title, author, year, genre, read_status, rating)
            st.success("🎉 Book added successfully!")
    
    elif choice == "🗑️ Remove a Book":
        st.subheader("🗑️ Remove a Book")
        title = st.text_input("📌 Enter book title to remove")
        if st.button("🗑️ Remove Book", help="Click to remove the book"):
            if remove_book(library, title):
                st.success("✅ Book removed successfully!")
            else:
                st.error("❌ Book not found!")
    
    elif choice == "🔍 Search for a Book":
        st.subheader("🔍 Search for a Book")
        query = st.text_input("📌 Enter book title or author")
        if st.button("🔎 Search", help="Click to search books"):
            results = search_books(library, query)
            if results:
                display_books(results)
            else:
                st.warning("❌ No matching books found.")
    
    elif choice == "📂 Display All Books":
        st.subheader("📚 All Books in Library")
        display_books(library)
    
    elif choice == "📊 Display Statistics":
        st.subheader("📊 Library Statistics")
        display_statistics(library)
    
if __name__ == "__main__":
    main()
