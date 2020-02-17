# CustomView

## Vòng đời của view

Cũng như Activity, Fragment View cũng có vòng đời của mình:
![Repo list](A6wI9Ld.png)

## Constructor

1. View(Context context)constructor này sẽ được sử dụng khi mà chúng ta add view lúc runtime.
2. View(Context context, AttributeSet attrs) constructor này sẽ được sử dụng khi chúng ta khai báo view trong XML (file layout xml á).
3. View(Context context, AttributeSet attrs, int defStyleAttr) cũng dùng trong XML nhưng thêm 1 tham số đó là các thuộc tính style của theme mặc định.
4. View(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) như cái 3 nhưng có thêm tham số để truyền style riêng thông qua resource.

## Attribute 

Để xác định các thành phần đó ta sử dụng cái AttributeSet ý, và khởi tạo nó qua file attrs.xml:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="MyView">
        <attr name="myview_radius" format="dimension" />
      

        <attr name="myview_colo" format="color" />
      
    </declare-styleable>
</resources>
```

## onMeasure

Hàm này dùng để xác định kích thước View:

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
      int widthMode = MeasureSpec.getMode(widthMeasureSpec);
      int widthSize = MeasureSpec.getSize(widthMeasureSpec);
      int heightMode = MeasureSpec.getMode(heightMeasureSpec);
      int heightSize = MeasureSpec.getSize(heightMeasureSpec);

      //desiredWidth: dựa vào nội dung muốn hiển thị mà bạn sẽ tính ra bạn cần tối thiểu bao nhiêu
      //không gian để bạn hiển thị
      ...
      int width;
      if (widthMode == MeasureSpec.EXACTLY) {
          width = widthSize;
      } else if (widthMode == MeasureSpec.AT_MOST) {
          width = Math.min(desiredWidth, widthSize);
      } else {
          width = desiredWidth;
      }
      ...
}
```

- MeasureSpec.EXACTLY: điều này nghĩa là chúng ta đã xác định cứng kích thước trong xml, như kiểu layout_width=300dp.
- MeasureSpec.AT_MOST: không nên vượt quá giới hạn này, vậy nên mới sử dụng câu lệnh Math.min(desiredWidth, widthSize).
- MeasureSpec.UNSPECIFIED: cho bạn thỏa sức, nhưng chúng ta chỉ cần những gì chúng ta thực sự cần mà thôi width = desiredWidth.

Sau khi view con tính toán xong việc nó cần kích thước như thế nào thì gọi đến method setMeasuredDimension để xác nhận, view cha sẽ nhận được thông tin đó và sẽ còn phải tính toán thêm vài lần nữa mới kết thúc.

## onLayout

Tại phương thức này thì mọi chuyện đã xong, kích thước đã được set cho tất cả các view con, lúc này chúng ta dùng lệnh getWidth, getHeight thì mới có giá trị, chứ ở các method trước chưa tính toán xong thì chỉ có = 0 

## onDraw

Đây là hàm mà ta có thể dùng canvas để vẽ

## View update
gọi hàm invalidate() sua khi thay đổi để update lại View

## Pain
