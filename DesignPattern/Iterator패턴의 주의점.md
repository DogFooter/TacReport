## 용도

- 컨테이너(데이터 집합)과 상관없이 요소에 대한 **처리를 반복**함
  &nbsp;
## 연관된 디자인 패턴  

- **Visitor 패턴** :  컨테이너의 요소를 하나씩 처리하는 Iterator 패턴과, 요소들을 방문하며 같은 처리를 반복하는  
  Visitor 패턴은 서로 비슷하지만 Iterator 패턴 안에는 처리 동작까지 기술되어있지는 않다 
  
- **Factory Method 패턴** : Iterator 인스턴스를 생성할 때
  Factory Method(상위클래스에서 생성 "방법"을 정의, 하위클래스에서 "생성")를 사용해서 생성하기도 함

## 사용시 주의점

- 정보의 주체가 아닌 **Iterator class가 요소의 정보를 받아서 처리하는 것은 좋지 않음**
  - 해당 정보의 주체가 되는 클래스가 직접 처리하게 할 것(자기 리소스는 자기가 처리)  

- 나쁜 예시 : 책장(BookShelf)의 정보를 가지고 있는 책장Iterator(BookShelfIterator)  
  - 남의 리소스를 가진 Iterator
  
```cpp

public class BookShelfIterator implements Iterator<Book>{
  private BookShelf bookShelf; // 자기 리소스가 아니므로 위험
  private int index;

public BookShelfIterator(BookShelf bookShelf) {
  this.bookShelf = bookShelf;
  this.index = 0;  
}

@Override
public boolean hasNext(){
  if(index < bookShelf.getLength()){
    return true;
  } else {
    return false;
  }
}

@Override
public Book next(){
  if(!hasNext()){
    throw new NoSuchElementException();    
  }
  Book book = bookShelf.getBookAt(index);
  index++;
return book;
  }
}


```
자기 리소스가 없는 BookShelfIterator를 BookShelf의 **내부 클래스**로 변경하여  
아래와 같이 구조를 바꿀 수 있다 
  &nbsp;
    &nbsp;
```cpp

public class BookShelf implements Iterable<Book> {

    private Book[] books;
    private int last = 0;

    public BookShelf(int maxsize) {
        this.books = new Book[maxsize];
    }

    public Book getBookAt(int index) {
        return books[index];
    }

    public void appendBook(Book book) {
        this.books[last] = book;
        last++;
    }

    public int getLength() {
        return last;
    }

    public Iterator<Book> iterator() { //Iterator 를 Bookshelf 내부로 이동

        return new Iterator<Book>() {
            private int index = 0;
            public boolean hasNext() {
                return index < last;
            }

            public Book next() {
                if(!hasNext())
                    throw new NoSuchElementException();
                return books[index++];
            }
        };
    }

}
```
