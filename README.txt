/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package rsaencryption;
import java.math.BigInteger;
import java.util.Scanner;
import java.util.Random;
import java.lang.Math;
/**
 *
 * @author bridg
 * This is the RSA encryption project for CISC2100.  
 */
public class RSAencryption {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        // TODO code application logic here
        		String message;
                        int p1;
                        int q1;
                        int m1;
                        
                System.out.println("This program tests the algorithm for RSA encryption and then implements RSA encryption to encrypt and decrypt a user entered message.");
                
                Scanner input1 = new Scanner(System.in);
                
                do{
                System.out.print("Enter a prime number: ");
                p1 = input1.nextInt();
                }while(isPrime(p1) != true);
                
                Scanner input2 = new Scanner(System.in);
                
                do{
                System.out.print("Enter another prime number: ");
                q1 = input2.nextInt();
                }while(isPrime(q1) != true);
                
                int n1 = p1*q1;
                
                int Xn = (p1 -1)*(q1-1);
                
                int e2 = RelativelyPrime(Xn);
                
                int d1 = inverse(e2, Xn);
               
                
                Scanner input3 = new Scanner(System.in);
                
                do{
                System.out.print("Enter a number smaller than " + n1 + ": ");
                m1 = input3.nextInt();
                }while(m1 >= n1);
                
                int encode = Encode(m1, e2, n1);
                
                System.out.println("The public key is (" + e2 + ", " + n1 + ")");
                System.out.println("The private key is (" + d1 + ", " + n1 + ")" + " Keep this number a secret!");
                
                int decode = Decode(encode, d1, n1);
                
                System.out.println("The encoded number is " + encode);
                System.out.println("The decoded number is " + decode);
		
		Scanner input = new Scanner(System.in);
		System.out.print("Enter a message to encrypt: ");
		message = input.nextLine();
		
		BigInteger [] M = new BigInteger[message.length()]; 	//message text
		M = MessagetoBigInteger(message);    //sets M to encrypted text
		
		//encryption
		int bitLength = 2048;
		Random rnd = new Random();
		Random rand = new Random();
		BigInteger p;
		BigInteger q;
		p = BigInteger.probablePrime(bitLength, rnd);
		q = BigInteger.probablePrime(bitLength, rand);
	        
		BigInteger n = p.multiply(q);

		BigInteger phi = (p.subtract(BigInteger.ONE)).multiply(q.subtract(BigInteger.ONE));
	        
		BigInteger e  = new BigInteger("65537");     // common value in practice = 2^16 + 1
	        
		BigInteger d = e.modInverse(phi);
	    
		System.out.println(" public key is  (" + e + ", " + n + ")"); 
	        
		System.out.println("\nThe encrypted message is: ");
	        
		for(int i = 0; i < M.length; i++){
			M[i] = M[i].modPow(e, n);
			System.out.print(M[i]);
	    }
	
		//Decryption
		BigInteger [] x = new BigInteger[message.length()];
		for (int i=0; i<x.length; i++) {
	    	x[i] = M[i].modPow(d,n);
	    }
	    	
	    System.out.println("\nThis is the retrieved message: ");
	    	
	    String decryptedmessage=null;
	    decryptedmessage=BigIntegertoString(x);
	    System.out.println("\n" +decryptedmessage);
	}

	public static String BigIntegertoString(BigInteger [] M) {
		char [] CharArray = new char[M.length];
	    int [] DigitArray = new int[M.length];
	    	
	    for(int i=0; i<M.length;i++) {
	    	DigitArray[i] = M[i].intValue();
	    	CharArray[i] = (char) DigitArray[i];
	    }	
	   	String s = new String(CharArray);
	    return s;
	}
	        
	public static BigInteger [] MessagetoBigInteger(String message) {
		char [] CharArray = null;
	    	
		CharArray = message.toCharArray();
	    int [] DigitArray = new int[message.length()];
	    	
	    BigInteger [] M = new BigInteger[CharArray.length];
	    	
	    for(int i=0; i<CharArray.length; i++) {
	    	DigitArray [i] = (int) CharArray[i];
	    	M[i] = BigInteger.valueOf(DigitArray[i]); 
	    	
	    }
	    return M;
	}
        
    public static boolean isPrime(int n) 
    { 
        // base case 
        if (n <= 1){
            return false;
        } 
      
        // Check from 2 to n-1 
        for (int i = 2; i < n; i++) {
            if (n % i == 0) {
                return false; }
        }
      
        return true; 
    }
        
        public static int EuclidAlgGCD(int a, int b){
            if(b == 0){
                return a;
            }
            if(a < b){
                return EuclidAlgGCD(b, a);
            }
            return EuclidAlgGCD(b, a % b);
        }
       public static int GCDExtended(int a, int b, int x, int y) { 
        // Base Case 
        if (a == 0) { 
            x = 0; 
            y = 1; 
            return b; 
        } 
  
        int x1 = 1, y1 = 1; // To store results of recursive call 
        int gcd = GCDExtended(b % a, a, x1, y1); 
  
        // Update x and y using results of recursive 
        // call 
        x = y1 - (b / a) * x1; 
        y = x1; 
  
        return gcd; 
        } 
        
        public static int mod(int a, int b){
            if(b > 0){
                if(a < 1){
                    a = (a*b)+(b-1);
                }
            }
            else{
                System.out.println("Invalid input.");
                return 0;
            }
            return a % b;
        }
        public static int RelativelyPrime(int n){
            int e;
            Random rand = new Random();
            do{
                e = rand.nextInt(1000);
            }while(EuclidAlgGCD(e, n) != 1);
            return e;
        }
        public static int inverse(int a, int b){
               BigInteger a1;
               BigInteger b1;
               a1 = BigInteger.valueOf((long)a);
               b1 = BigInteger.valueOf((long)b);
               
               BigInteger result;
               result = a1.modInverse(b1);
               
               int d = result.intValue();
               
               return d;
             
        }
        
    public static int Encode (int M, int e, int PQ){
            BigInteger M1;
            BigInteger e1;
            BigInteger pq1;
            M1 = BigInteger.valueOf((long) M);
            e1 = BigInteger.valueOf((long)e);
            pq1 = BigInteger.valueOf((long)PQ);
            
            BigInteger result;
            
            result = M1.modPow(e1, pq1);
            
            int encode = result.intValue();
            
            return encode;
        }
        public static int Decode (int C, int d, int PQ){
            BigInteger C1;
            BigInteger d1;
            BigInteger pq1;
            C1 = BigInteger.valueOf((long) C);
            d1 = BigInteger.valueOf((long)d);
            pq1 = BigInteger.valueOf((long)PQ);
            
            BigInteger result;
            
            result = C1.modPow(d1, pq1);
            
            int decode = result.intValue();
            
            return decode;
        }
        
}
  