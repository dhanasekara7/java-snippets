```java
// FileFilter
public class JavaFileFilter implements FileFilter {

    public boolean accept(File file) {
        return file.getName().endsWith(".java");
    }

    public static void main(String[] args) {
        final JavaFileFilter javaFileFilter = new JavaFileFilter();
        File dir = new File("/user/dhans/java/");
        File[] files = dir.listFiles(javaFileFilter);
    }
}

```