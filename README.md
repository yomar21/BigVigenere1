# BigVigenere1
lab eda



import java.util.Scanner;

public class BigVigenere {

    private int[] key;
    private char[][] alphabet;
    private  String ALPHABET = "ABCDEFGHIJKLMÑNOPQRSTUVWXYZabcdefghijklmnñopqrstuvwxyz0123456789";
    private int a=ALPHABET.length();
    public BigVigenere(String numericKey) {
        this.key = parseKey(numericKey);
        generateAlphabetMatrix();
    }

    private int[] parseKey(String numericKey) {
        int[] keyArray = new int[numericKey.length()];
        for (int i = 0; i < numericKey.length(); i++) {
            keyArray[i] = Character.getNumericValue(numericKey.charAt(i));
        }
        return keyArray;
    }

    private void generateAlphabetMatrix() {

        alphabet = new char[a][a];
        for (int i = 0; i < a; i++) {
            for (int j = 0; j < a; j++) {
                alphabet[i][j] = ALPHABET.charAt((i + j) % a);

            }
        }
    }

    public String encrypt(String mensaje) {

        String mensajeencriptado = "";
        for (int i = 0; i < mensaje.length(); i++) {
            char originalChar = mensaje.charAt(i);
            int indice = ALPHABET.indexOf(originalChar);
            if (indice == -1) {
                mensajeencriptado+=originalChar;
                continue;
            }
            int indicekey = key[i % key.length];
            mensajeencriptado+=alphabet[indice][indicekey];
        }

        return mensajeencriptado;

    }

    public String decrypt(String mensajeencriptado) {

        String mensajeDesencriptado = "";
        for (int i = 0; i < mensajeencriptado.length(); i++) {
            char letraencriptada =mensajeencriptado.charAt(i);
            int keyIndice = key[i % key.length];
            int originalIndice = -1;
            for (int j = 0; j < ALPHABET.length(); j++) {
                if (alphabet[j][keyIndice] == letraencriptada) {
                    originalIndice = j;
                    break;
                }
            }
            mensajeDesencriptado += (originalIndice == -1 ? letraencriptada : ALPHABET.charAt(originalIndice));

        }

        return mensajeDesencriptado;
    }


    public char optimalSearch(int fila, int columna) {
        if (fila < 0 || fila >= a || columna < 0 || columna >= a) {
            throw new IllegalArgumentException("Fila o columna fuera de rango.");
        }
        return alphabet[fila][columna];
    }


    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Ingrese la clave numérica: ");
        String key = scanner.nextLine();
        BigVigenere mivigenere = new BigVigenere(key);

        System.out.print("Ingrese el mensaje a encriptar: ");
        String mensaje = scanner.nextLine();
        long millisini=System.currentTimeMillis()  ;
        String encrypted = mivigenere.encrypt(mensaje);
        long millisfin=System.currentTimeMillis() ;
        long tiempo=millisfin-millisini;
        System.out.println("Mensaje encriptado: " + encrypted);
        System.out.println("tiempo de ejecucion de emcriptado en milisegundo: " + tiempo );


        long tiempoinicio=System.currentTimeMillis() % 1000 ;
        String decrypted = mivigenere.decrypt(encrypted);
        long tiempofin=System.currentTimeMillis() % 1000 ;
        long tiempo2 =tiempofin-tiempoinicio;
        System.out.println("tiempo de ejecucion de desemcriptado en milisegundo: " + tiempo2 );

        System.out.println("Mensaje desencriptado: " + decrypted);

    }


}

