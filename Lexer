import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/*
 * Class Lexer
 * 
 * Update the method nextToken() such to the provided
 * specifications of the Decaf Programming Language.
 * 
 * You are not allowed to use any built it in tokenizer
 * in Java. You are only allowed to scan the input file
 * one character at a time.
 */

public class Lexer {

	private BufferedReader reader; // Reader
	private char curr; // The current character being scanned

	private static final char EOF = (char) (-1);

	// End of file character

	public Lexer(String file) {
		try {
             	reader = new BufferedReader(new FileReader(file));
		} catch (Exception e) {
			e.printStackTrace();
		}

		// Read the first character
		curr = read();
	}

	private char read() {
		try {
			return (char) (reader.read());
		} catch (IOException e) {
			e.printStackTrace();
			return EOF;
		}
	}

	// Checks if a character is a digit
	private boolean isNumeric(char c) {
		if (c >= '0' && c <= '9')
			return true;

		return false;
	}

	public boolean isAlpha(char c){
		if(c>='a' && c<='z' )
		return true;
		if(c>='A' && c<='Z' )
		return true;	
		
		return false;

	}

	public Token nextToken() {

		int state = 1; // Initial state
		int numBuffer = 0; // A buffer for number literals
		String alphaBuffer = "";
		int decBuffer=0;
                boolean skipped = false;
		while (true) {
		if (curr == EOF && !skipped) {
                        skipped = true;
                      
                }else if (skipped) {
                            
                            try {
                            
                                    reader.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
                                
				return null;
			}
				
			switch (state) {
			// Controller
			case 1:
				switch (curr) {
				case ' ': // Whitespaces
				case '\n':
				case '\b':
				case '\f':
				case '\r':
				case '\t':	
					curr = read();
					continue;

				case ';':
					curr = read();
					return new Token("SM", ";");

				case '+':
					curr = read();
					return new Token("PO", "+");

				case '-':
					curr = read();
					return new Token("MO", "-");

				case '*':
					curr = read();
					return new Token("TO", "*");

				case '/':
					curr = read();
                                        state = 14;
                                        continue;
					//return new Token("DO", "/");
				case',':
					curr=read();	
					return new Token("FA",",");
				case'(':
					curr=read();	
					return new Token("LP","(");
				case')':
					curr=read();	
					return new Token("RP",")");
				case'{':
					curr=read();	
					return new Token("LB","{");
				case'}':
					curr=read();	
					return new Token("RB","}");
				case'%':
					curr=read();	
					return new Token("MD","%");
				case'=':
					curr=read();	
					state=8;
					continue;
					
				case'!':
					curr=read();
					state=9;
					continue;
				case'&':
					curr=read();
					state=10;
					continue;
				case'|':
					curr=read();
					state=11;
					continue;	 
                                case '"':
                                        curr=read();
                                        state=13;
                                        alphaBuffer="";
                                        continue;
                             
				default:
					state = 2; // Check the next possibility
					continue;
				}

				// Integer - Start
			case 2:
				if (isNumeric(curr)) {
					numBuffer = 0; // Reset the buffer.
					numBuffer += (curr - '0');

					state = 3;
                                     
					curr = read();
                                       
				} else {
					state=5; //doesnot start with number or symbol go to case 5
				}
				continue;

				// Integer - Body
			case 3:
				if (isNumeric(curr)) {
					numBuffer *= 10;
					numBuffer += (curr - '0');

					curr = read();
                                        
				}else if(curr=='.'){
					
					curr = read();	
                                       
					state=4; //has decimal point go to case 4
                                        
                                        } else {
					return new Token("NM", "" + numBuffer);
				}
                                
				continue;
				
				//decimal-start	
			case 4:
				if (isNumeric(curr)) {
					decBuffer = 0;
					decBuffer += (curr - '0');
					state=7;					
					curr = read();	
                                        
				}else  {
					return new Token("ERROR", "Invalid input: "+numBuffer+"." );
				}
				continue;
				//decimal body
			case 7:
				if (isNumeric(curr)) {
					decBuffer *= 10;
					decBuffer += (curr - '0');

					curr = read();
				} else {
					return new Token("NM", "" + numBuffer+"."+decBuffer);
				}
				continue;	

				//identifier -start
			case 5:
				if(isAlpha(curr)|| curr=='_'){
				alphaBuffer = "";					
				alphaBuffer+=curr;
				state=6;
				curr = read();
				}else {
                                    alphaBuffer = "";					
				    alphaBuffer+=curr;
                                        curr=read();
					return new Token("ERROR", "Invalid input:"+alphaBuffer);
				}
				continue;	
			
				//identifier - Body
			case 6:	
				if ((isAlpha(curr) || isNumeric(curr) || curr=='_')) {
					
					alphaBuffer += curr;
					curr = read();
                                       
                                        
				} else {
                                    
                                    if( alphaBuffer.equals("class")||
                                        alphaBuffer.equals("static")||   
                                        alphaBuffer.equals("else")||
                                        alphaBuffer.equals("if")||
                                        alphaBuffer.equals("int")||
                                        alphaBuffer.equals("float")||
                                        alphaBuffer.equals("boolean")||
                                        alphaBuffer.equals("String")||
                                        alphaBuffer.equals("return")||
                                        alphaBuffer.equals("while")   
                                            ){
                                        return new Token("KW", "" + alphaBuffer);
                                        
                                    }else if(alphaBuffer.equals("true")||alphaBuffer.equals("false"))
                                    {
                                        return new Token("BL", "" + alphaBuffer);
                                    }
                                    
                                    
					return new Token("ID", "" + alphaBuffer);
				}
				continue;
				
				// if ==
			case 8: 
				if(curr=='=')	{
					curr=read();
					return new Token("EQ","==");
				}
				else {
					
					return new Token("AO","=");		
				}
				//if !=
			case 9: 
				if(curr=='=')	{
					curr=read();
					return new Token("NE","!=");
				}
				else {
					return new Token("ERROR", "Invalid input: !");
				}

			// if &&
			case 10: 
				if(curr=='&')	{
					curr=read();
					return new Token("LA","&&");
				}
				else {
					return new Token("ERROR", "Invalid input: &");
				}
			// if || 
			case 11: 
				if(curr=='|')	{
					curr=read();
					return new Token("LO","||");
				}
				else {
					return new Token("ERROR", "Invalid input: |");
				}
                            
                        case 13:
                            if(curr=='"'){
                                curr=read();
                                return new Token("ST","\""+alphaBuffer+"\"");
                            }
                            else if(curr=='\n' || curr==EOF){
                                curr=read();
                                return new Token("ERROR","Invalid string literal"); 
                            }
                            else{
                                alphaBuffer += curr;
                                curr = read();
                            }
                            continue;
                            //alphaBuffer += curr;
                            //curr = read();
                         
                        case 14:
                            if(curr=='/'){
                                 state = 15;
                                 curr=read();
                            }else if(curr=='*')
                            {
                                state = 16;
                                curr=read();
                            }
                            else{
                                return new Token("DO", "/");
                            }
                            continue;
                        case 15:
                             if(curr=='\n'){
                                 
                                  state = 1;
                             }
                            curr=read();
                            continue;
                        case 16:
                            if(curr=='*')
                            {
                                state = 17;
                               
                            }
                             curr=read();
                            continue;
                        case 17:
                            if(curr=='/'){
                                curr=read();
                                state = 1;
                            }
                            else{
                                curr=read();
                                state=16;
                            }
                            continue;
                       
			}
		}
	}
}
