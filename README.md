# Implementing Git Hooks in the PHP Project

This project includes an example of how to implement a Git hook called pre-commit that checks the syntax of PHP files before allowing a commit to be made.

## Steps to Configure the Pre-Commit Hook

1. Locate the Hooks Folder

  1. Verify that the hooks folder exists in the local repository:

     ```bash
        ls -a .git/hooks 
     ```
     
     You should see files like pre-commit.
     
  2. The content of the file is as follows:
     
     ```bash
        #!/bin/bash
        echo "Verificando sintaxis de archivos PHP..."
        
        # Busca archivos PHP en el área de staging
        for file in $(git diff --cached --name-only | grep '\.php$'); do
          # Verifica la sintaxis de cada archivo PHP
          php -l $file
          if [ $? -ne 0 ]; then
            echo "Errores de sintaxis detectados en $file. Commit bloqueado."
            exit 1
          fi
        done
        
        echo "Sintaxis válida. Commit permitido." 
     ```

## Testing the Hook

1. Introduces a syntax error into a PHP file (for example, removes a semicolon ;):

     ```bash
        <?php
        echo "Hola Mundo usando Git Hooks" // Error: Falta el punto y coma
        ?>
     ```

2. Try making a commit:

   ```bash
      git add index.php
      git commit -m "Probar hook pre-commit"
   ```

3. If there is a syntax error, the hook will block the commit and display a message like:

   ```bash
      Verificando sintaxis de archivos PHP...
      PHP Parse error:  syntax error, unexpected end of file in index.php on line X
      Errores de sintaxis detectados en index.php. Commit bloqueado.
   ```

4. Fix the error, re-add the changes, and commit again:

   ```bash
      git add index.php
      git commit -m "Probar hook pre-commit"
   ```
   
   This time the commit will be allowed if there are no errors.
