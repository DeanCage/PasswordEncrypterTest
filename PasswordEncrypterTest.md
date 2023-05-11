import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Base64.*;

public class PasswordEncrypter {
    // зашифрованный пароль
    public static byte[] encrypt (String password, String algorithm) throws IllegalArgimentException {
        try {
            MessageDigest d = MessageDigest.getInstance(algorithm);
            d.update(password.getBytes());
            return d.digest();
        } catch (NoSuchAlgorithmException nsae) {
           throw new IllegalArgumentException("Illegal algorithm value.");
        }
    } // encrypt(String, String)
    // шифровка хэш-ключа
    public static String encryptAndEncode(String password, String algorithm)
        throws IllegalArgumentException {
        BASE64Encoder enc = new BASE64Encoder();
        String encoded = enc.encode(encrypt(password, algorithm));
        enc = null;
        return encoded;
    } // encryptAndEncode (String, String)

    private static class IllegalArgimentException extends Exception {
    }
} // PasswordEncrypter

public class PasswordTest {
    public static void main(String[] args) {
        String pwd = "пароль";
        String encrypted = new String(PasswordEncrypter.encrypt(pwd));
        String encoded = PasswordEncrypter.encryptAndEncode(pwd);

        System.out.println("Зашифрованный пароль:" + encrypted);
        System.out.println("Кодированый шифр:" + encoded);
    }
}

