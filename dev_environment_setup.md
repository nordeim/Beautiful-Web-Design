Okay, let's break down the steps to prepare your development environment on Ubuntu 24.04 and Windows 10/11, based on the technology stack we discussed (Node.js, Next.js, PostgreSQL, Prisma, Stripe CLI, Git).

**I. Common Prerequisite: Code Editor**

A good code editor is essential. Visual Studio Code (VS Code) is highly recommended and works well on both Linux and Windows.

*   **Download:** Get it from the official website: [https://code.visualstudio.com/](https://code.visualstudio.com/)

**II. Ubuntu 24.04 LTS Setup**

Open your terminal for these commands.

1.  **Update Package List:**
    ```bash
    sudo apt update
    ```

2.  **Install Git (Version Control):**
    *(Often pre-installed, but good to ensure)*
    ```bash
    sudo apt install git -y
    git --version
    ```

3.  **Install Node.js (using NVM - Node Version Manager - Recommended):**
    *   NVM allows you to easily switch between Node.js versions.
    *   **Install curl (if not present):**
        ```bash
        sudo apt install curl -y
        ```
    *   **Install NVM:** (Check the official NVM GitHub page for the *very latest* install script URL)
        ```bash
        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
        ```
    *   **Load NVM:** Close and reopen your terminal, or run:
        ```bash
        export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
        ```
        *(You might need to add the above lines to your shell profile like `~/.bashrc` or `~/.zshrc` if nvm isn't found after restarting the terminal)*
    *   **Install Node.js LTS (Long Term Support):**
        ```bash
        nvm install --lts
        ```
    *   **Verify Installation:**
        ```bash
        node -v
        npm -v
        ```

4.  **Install PostgreSQL (Database Server & Client):**
    ```bash
    sudo apt install postgresql postgresql-contrib -y
    ```
    *   **Start and Enable the Service:**
        ```bash
        sudo systemctl start postgresql
        sudo systemctl enable postgresql # Ensures it starts on boot
        ```
    *   **Verify Status:**
        ```bash
        sudo systemctl status postgresql # Press 'q' to exit status view
        ```
    *   **Initial Database Setup (Create User and Database for your project):**
        *   Switch to the `postgres` system user:
            ```bash
            sudo -i -u postgres
            ```
        *   Open the PostgreSQL prompt:
            ```bash
            psql
            ```
        *   Inside `psql`, run these SQL commands (replace `your_db_user`, `your_password`, and `your_db_name`):
            ```sql
            CREATE USER your_db_user WITH PASSWORD 'your_password';
            CREATE DATABASE your_db_name OWNER your_db_user;
            GRANT ALL PRIVILEGES ON DATABASE your_db_name TO your_db_user;
            \q  -- Exit psql
            ```
        *   Return to your regular user:
            ```bash
            exit
            ```

5.  **Install Stripe CLI (for testing webhooks locally):**
    *   Follow the official Stripe documentation for Linux: [https://stripe.com/docs/stripe-cli#install-linux](https://stripe.com/docs/stripe-cli#install-linux)
    *   Typically involves adding their repository and installing via `apt`:
        ```bash
        # Example commands (VERIFY on Stripe's site first!)
        curl -s https://packages.stripe.dev/api/security/keypair/stripe-cli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/stripe-cli-archive-keyring.gpg
        echo "deb [signed-by=/usr/share/keyrings/stripe-cli-archive-keyring.gpg] https://packages.stripe.dev/stripe-cli-stable main" | sudo tee /etc/apt/sources.list.d/stripe-cli.list
        sudo apt update
        sudo apt install stripe -y
        ```
    *   **Verify Installation:**
        ```bash
        stripe --version
        ```
    *   **Login to Stripe (links your CLI to your Stripe account):**
        ```bash
        stripe login
        ```

**III. Windows 10 / 11 Setup**

Use Command Prompt (cmd) or PowerShell (run as Administrator where needed).

1.  **Install Git:**
    *   **Download:** Get the installer from [https://git-scm.com/download/win](https://git-scm.com/download/win).
    *   **Install:** Run the installer. Accept defaults, but ensure options like "Git Bash Here", "Git GUI Here", and importantly **"Git from the command line and also from 3rd-party software"** (or similar option adding Git to PATH) are selected.
    *   **Verify:** Open a *new* Command Prompt or PowerShell and run:
        ```powershell
        git --version
        ```

2.  **Install Node.js:**
    *   **Download:** Get the Windows Installer (.msi) LTS version from [https://nodejs.org/](https://nodejs.org/).
    *   **Install:** Run the installer. Accept defaults; ensure the option to "Add to PATH" is checked. It might also offer to install necessary tools like Chocolatey - you can allow this.
    *   **Verify:** Open a *new* Command Prompt or PowerShell and run:
        ```powershell
        node -v
        npm -v
        ```

3.  **Install PostgreSQL:**
    *   **Download:** Get the installer from EnterpriseDB (EDB), the official community distributor: [https://www.enterprisedb.com/downloads/postgres-postgresql-downloads](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads). Select your Windows version.
    *   **Install:** Run the installer.
        *   Follow the prompts. Choose an installation directory.
        *   Select components: At minimum, select "PostgreSQL Server", "Command Line Tools". "pgAdmin 4" is a useful GUI tool you'll likely want. Stack Builder is optional.
        *   Choose a data directory or accept the default.
        *   **Set a password for the `postgres` superuser.** Remember this password!
        *   Accept the default port (5432) unless you have a conflict.
        *   Accept the default locale.
    *   **Add PostgreSQL bin to PATH (Crucial!):**
        *   Find the `bin` directory within your PostgreSQL installation path (e.g., `C:\Program Files\PostgreSQL\16\bin` - version number might differ).
        *   Search for "Environment Variables" in the Windows search bar and select "Edit the system environment variables".
        *   Click "Environment Variables...".
        *   Under "System variables", find the `Path` variable, select it, and click "Edit...".
        *   Click "New" and paste the full path to your PostgreSQL `bin` directory.
        *   Click OK on all dialogs.
    *   **Verify:** Open a *new* Command Prompt or PowerShell and run:
        ```powershell
        psql --version
        ```
    *   **Initial Database Setup (Using pgAdmin or psql):**
        *   **Using pgAdmin:** Launch pgAdmin. Connect to the server (using the password you set during installation). In the object browser, right-click "Login/Group Roles" -> "Create" -> "Login/Group Role...". Enter `your_db_user`, go to the "Definition" tab, enter `your_password`. Go to the "Privileges" tab, enable "Can login?". Save. Then, right-click "Databases" -> "Create" -> "Database...". Enter `your_db_name`, set the "Owner" to `your_db_user`. Save.
        *   **Using psql (alternative):** Open Command Prompt/PowerShell. Connect using the postgres superuser:
            ```powershell
            psql -U postgres
            ```
            Enter the superuser password you set during installation. Then run the SQL commands:
            ```sql
            CREATE USER your_db_user WITH PASSWORD 'your_password';
            CREATE DATABASE your_db_name OWNER your_db_user;
            GRANT ALL PRIVILEGES ON DATABASE your_db_name TO your_db_user;
            \q
            ```

4.  **Install Stripe CLI:**
    *   **Download:** Go to the Stripe CLI releases page on GitHub: [https://github.com/stripe/stripe-cli/releases/latest](https://github.com/stripe/stripe-cli/releases/latest). Download the `.exe` file for Windows (e.g., `stripe_X.Y.Z_windows_x86_64.exe`).
    *   **Install:** Rename the downloaded `.exe` to `stripe.exe`. Place it in a directory that is included in your system's PATH, or create a dedicated directory (e.g., `C:\stripe`) and add *that* directory to your system PATH (using the same method as for PostgreSQL).
    *   **Verify:** Open a *new* Command Prompt or PowerShell and run:
        ```powershell
        stripe --version
        ```
    *   **Login to Stripe:**
        ```powershell
        stripe login
        ```

**(Optional but Recommended for Windows) Windows Subsystem for Linux (WSL):**

Many developers prefer using WSL on Windows for a Linux-like development experience.

1.  Open PowerShell as Administrator.
2.  Run: `wsl --install` (This installs Ubuntu by default).
3.  Restart your computer when prompted.
4.  After restarting, Ubuntu will launch and complete installation, asking you to create a UNIX username and password.
5.  Once set up, you can open the Ubuntu terminal and follow the **Ubuntu 24.04 Setup** steps above *within* the WSL environment. VS Code integrates very well with WSL.

**IV. Initial Project Setup (After Environment is Ready)**

These steps are the same for Ubuntu, Windows (native), or Windows (WSL). Open your terminal or command prompt in the directory where you want to create your project.

1.  **Create Next.js Project:**
    *   Use `create-next-app`. We'll enable TypeScript (`--ts`) and Tailwind CSS (`--tailwind`). Answer the prompts (like enabling App Router, ESLint).
    ```bash
    npx create-next-app@latest your-project-name --ts --tailwind --eslint --app --src-dir --use-npm --import-alias "@/*"
    cd your-project-name
    ```
    *(Adjust flags based on your preferences if different from the blueprint's Next.js/Tailwind)*

2.  **Install Prisma:**
    ```bash
    npm install prisma --save-dev
    ```

3.  **Initialize Prisma:**
    *   This creates `prisma/schema.prisma` and a `.env` file.
    ```bash
    npx prisma init --datasource-provider postgresql
    ```

4.  **Configure Database Connection (`.env` file):**
    *   Open the `.env` file created in the project root.
    *   Modify the `DATABASE_URL` line using the user, password, and database name you created earlier:
    ```env
    # .env
    DATABASE_URL="postgresql://your_db_user:your_password@localhost:5432/your_db_name"
    ```
    *(Ensure the user, password, host (localhost), port (5432), and database name are correct)*

5.  **Define Prisma Schema:**
    *   Open `prisma/schema.prisma`.
    *   Replace its contents with the schema provided in the blueprint (Section 4 of the previous response).

6.  **Run Initial Database Migration:**
    *   This command creates the actual tables in your PostgreSQL database based on your schema.
    ```bash
    npx prisma migrate dev --name init
    ```
    *   Prisma will prompt you to name the migration (we provided `init`). It applies the changes and generates the Prisma Client.

7.  **Install Other Dependencies:**
    ```bash
    npm install stripe
    npm install next-auth # If using NextAuth.js for authentication
    npm install bcrypt jsonwebtoken
    npm install @types/bcrypt @types/jsonwebtoken --save-dev # Types for TypeScript
    ```

8.  **Run Development Server:**
    ```bash
    npm run dev
    ```
    Your Next.js app should now be running (usually at `http://localhost:3000`).

----
https://aistudio.google.com/app/prompts?state=%7B%22ids%22:%5B%221fcSYESkb861UCp6YUiqKC8RONf53Vyyq%22%5D,%22action%22:%22open%22,%22userId%22:%22103961307342447084491%22,%22resourceKeys%22:%7B%7D%7D&usp=sharing,  
https://drive.google.com/file/d/1yAXwHmTw0ojGMjZMRfilZzRTq4nLW4O_/view?usp=sharing

Your development environment is now set up, and your project has the basic structure, database connection configured, and initial tables created. You can start building the components and API routes outlined in the blueprint.
