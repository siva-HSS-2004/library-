import mysql.connector
from mysql.connector import Error

def create_connection():
    try:
        connection = mysql.connector.connect(
            host='localhost',
            user='root',
            password='',
            database='library_db'
        )
        return connection
    except Error as e:
        print(f"Error: {e}")
        return None

def add_book(title, author, year):
    connection = create_connection()
    if connection is None:
        return
    cursor = connection.cursor()
    query = "INSERT INTO books (title, author, year) VALUES (%s, %s, %s)"
    cursor.execute(query, (title, author, year))
    connection.commit()
    cursor.close()
    connection.close()

def view_books():
    connection = create_connection()
    if connection is None:
        return
    cursor = connection.cursor()
    cursor.execute('SELECT * FROM books')
    books = cursor.fetchall()
    for book in books:
        print(f"ID: {book[0]}, Title: {book[1]}, Author: {book[2]}, Year: {book[3]}")
    cursor.close()
    connection.close()

def update_book(book_id, title, author, year):
    connection = create_connection()
    if connection is None:
        return
    cursor = connection.cursor()
    query = "UPDATE books SET title = %s, author = %s, year = %s WHERE id = %s"
    cursor.execute(query, (title, author, year, book_id))
    connection.commit()
    cursor.close()
    connection.close()

def delete_book(book_id):
    connection = create_connection()
    if connection is None:
        return
    cursor = connection.cursor()
    query = "DELETE FROM books WHERE id = %s"
    cursor.execute(query, (book_id,))
    connection.commit()
    cursor.close()
    connection.close()

def main_menu():
    while True:
        print("\nLibrary Management System")
        print("1. Add Book")
        print("2. View Books")
        print("3. Update Book")
        print("4. Delete Book")
        print("5. Exit")
        
        choice = input("Enter your choice: ")
        
        if choice == '1':
            title = input("Enter book title: ")
            author = input("Enter book author: ")
            year = int(input("Enter book year: "))
            add_book(title, author, year)
        elif choice == '2':
            view_books()
        elif choice == '3':
            book_id = int(input("Enter book ID to update: "))
            title = input("Enter new book title: ")
            author = input("Enter new book author: ")
            year = int(input("Enter new book year: "))
            update_book(book_id, title, author, year)
        elif choice == '4':
            book_id = int(input("Enter book ID to delete: "))
            delete_book(book_id)
        elif choice == '5':
            break
        else:
            print("Invalid choice, please try again.")

if __name__ == '__main__':
    main_menu()
