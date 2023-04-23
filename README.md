# Stack-HTML-Renderer
import java.util.*;

public class HTML_Renderer {
    public static void main(String[] args) {
        Scanner inputUser = new Scanner(System.in);
        ArrayList<String> tagContentList = new ArrayList<>();
        ArrayStack tagStack = new ArrayStack();
        while(true) {
            String input = inputUser.nextLine();


            if (input.equals("<!-- end -->")) {

                if ( tagStack.isEmpty()) {
                    for (Object tagContent : tagContentList) {
                        System.out.println(tagContent);
                    }
                } else if (tagContentList.isEmpty() || !tagStack.isEmpty()){
                    System.out.println("dokumen tidak valid");
                }
                break;

            }
            else if (input.equals("</>") ||input.matches("</.*>") ) {

                if (tagStack.isEmpty()) {
                    System.out.println("dokumen tidak valid");
                    break;

                } else if (input.substring(2, input.length() - 1).equalsIgnoreCase(tagStack.peek().toString())) {
                    tagStack.pop();
                } else {
                    System.out.println("dokumen tidak valid");
                    break;
                }
            }

            else if (input.matches("<.*>") || input.equals("<>")) {
                tagStack.push(input.substring(1, input.length() - 1).toLowerCase());
            }

            else  {
                if (tagStack.isEmpty()) {
                    continue;
                }
                tagContentList.add(input);
            }
        }
    }
}

class ArrayStack {
    private int top = -1;
    private final int ARRAY_SIZE = 5;
    private Object[] stack = new Object[ARRAY_SIZE];

    public boolean isEmpty() {
        return top == -1;
    }

    private void resize(int size) {
        Object[] newStack = new Object[size];
        for (int i = 0; i <= top; i++) {
            newStack[i] = stack[i];
        }
        stack = newStack;
    }

    public void push(Object value) {
        if (top + 1 == stack.length) {
            resize(stack.length * 2);
        }
        stack[++top] = value;
    }

    public Object pop() {
        if (isEmpty()) {
            return null;
        }
        Object tmp = stack[top];
        stack[top--] = null;
        if (top < stack.length/4 && stack.length > ARRAY_SIZE) {
            resize(stack.length/2);
        }
        return tmp;
    }

    public Object peek() {
        if (isEmpty()) {
            return null;
        }
        return stack[top];
    }

    public void print() {
        if (isEmpty()) {
            System.out.println("null");
            return;
        }
        for (int i = 0; i <= top; i++) {
            System.out.print(stack[i] + " ");
        }
        System.out.println();
    }
}
