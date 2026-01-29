# POS Next
```
https://github.com/BrainWise-DEV/POSNext
```
<img width="802" height="301" alt="image" src="https://github.com/user-attachments/assets/abefa7fb-56fc-4e15-b0d5-46c6b55fc14d" />


---

Here’s a clean “from zero to working POSNext on ERPNext v15” checklist (Ubuntu 24.04 / frappe-bench). This is the setup that avoids the Node 18 vs 20 problem you hit.

0) Pre-reqs (system packages)
```
sudo apt update
sudo apt install -y git curl build-essential python3-dev python3-venv redis-server mariadb-server mariadb-client supervisor cron
```

(If your bench is already running, you probably have most of these.)

1) Install NVM (Node Version Manager)

As the erpnext user:
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm -v
```
2) Install Node 20 + set default (important for POSNext)
```
nvm install 20
nvm use 20
nvm alias default 20
```
```
node -v   # should be v20.x
npm -v
```

Why: POSNext frontend deps require Node >= 20 (that’s the error you had).

3) Install Yarn (use Corepack, best with Node 20)
```
corepack enable
corepack prepare yarn@1.22.22 --activate
yarn -v
```

4) Make sure Supervisor services also use Node 20 (NVM path)

Interactive shell uses Node 20 now, but services might not.

Get Node 20 binary path:
```
nvm which 20
```

Add Node 20 bin to Supervisor program environment PATH.

Find bench supervisor config:
```
sudo grep -R "frappe-bench" -n /etc/supervisor/conf.d
```

Edit the relevant conf file(s) and add to each program: block that runs node:

environment=PATH="/home/erpnext/.nvm/versions/node/v20.x.x/bin:%(ENV_PATH)s"


Reload supervisor:

```
sudo supervisorctl reread
sudo supervisorctl update
```

5) Bench sanity checks (ERPNext v15)

From ~/frappe-bench:

```
cd ~/frappe-bench
bench version
node -v
yarn -v
```

6) Install POSNext (the successful flow)
cd ~/frappe-bench

#### (optional but recommended) remove any partial install first

```
rm -rf apps/pos_next
```

```
bench get-app https://github.com/BrainWise-DEV/POSNext
bench --site site1.local install-app pos_next
```

```
bench build
bench --site site1.local migrate
bench restart
```

7) After install verification

```
bench --site site1.local list-apps | grep pos_next
```


And in the UI, confirm POSNext module / pages appear.

Optional but recommended “updates”
Update bench (only if you want—do this carefully on production)

You saw:
bench 5.25.9 → 5.29.0

If you update bench, do it during a maintenance window:

```
pipx upgrade frappe-bench
bench --version
```

Summary of “what you need”

nvm (to manage Node versions)
node v20.x (POSNext requirement)
npm (comes with Node)
yarn 1.22.x (via corepack)
Usual ERPNext stack packages: git/curl/build-essential/python3-dev/redis/mariadb/supervisor

If you tell me whether your bench is production (supervisor) or development (bench start), I’ll tailor the “Supervisor PATH/NVM” step exactly for your setup so it never flips back to Node 18 again.

---
---
# For Development & Production

Here’s the complete step-by-step for both:
Development: you run bench start (interactive shell)
Production: you run via Supervisor (services)
This is written specifically to avoid the Node 18 vs Node 20 mismatch you hit while installing POSNext.

A) One-time base requirements (common to dev + prod)
1) System packages
```
sudo apt update
sudo apt install -y git curl build-essential python3-dev python3-venv redis-server mariadb-server mariadb-client supervisor cron
```
2) Bench folder

Assuming you already have ~/frappe-bench. If not, you’d create it via bench init etc. (you already have it).

B) Node / npm / yarn setup (the key for POSNext)
1) Install NVM (as erpnext user)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
```
```
nvm -v
```
2) Install Node 20 and make it default
```
nvm install 20
nvm use 20
nvm alias default 20
```
```
node -v   # v20.x
npm -v
```
3) Install Yarn (recommended: Corepack)
```
corepack enable
corepack prepare yarn@1.22.22 --activate
```
```
yarn -v
```

### ✅ At this point your development shell is ready.

# C) Development mode (bench start)
1) Use Node 20 before you run bench

Every time you open a new shell, confirm:
```
node -v
```

If it’s not v20:
```
nvm use 20
```

2) Run bench in dev mode
```
cd ~/frappe-bench
bench start
```

That’s it for development.

# D) Production mode (Supervisor) — ensure Node 20 for services

This is the critical part: Supervisor does not load your ~/.bashrc, so it won’t automatically use NVM’s Node 20 unless you set it.

1) Get the Node 20 path
```
nvm which 20
```

Example output:
/home/erpnext/.nvm/versions/node/v20.11.1/bin/node

So the bin directory is:
/home/erpnext/.nvm/versions/node/v20.11.1/bin

2) Find your Supervisor config for this bench
```
sudo grep -R "frappe-bench" -n /etc/supervisor/conf.d
```

Open the matching file (commonly something like):

/etc/supervisor/conf.d/frappe-bench.conf
or

/etc/supervisor/conf.d/frappe-bench-*.conf

3) Add Node 20 to PATH for node programs

In that conf file, locate program blocks like:

frappe-bench-node-socketio

(sometimes) frappe-bench-web / frappe-bench-workers

Add environment line to the node-socketio block (and any other block that invokes node tools):

environment=PATH="/home/erpnext/.nvm/versions/node/v20.11.1/bin:%(ENV_PATH)s"


(Replace v20.11.1 with your actual version directory.)

4) Reload supervisor and restart bench services
```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl restart all
```

or just:
```
bench restart
```

5) Verify running processes are using Node 20
   
```
ps aux | grep node
```

You should see node paths containing .../v20.../bin/node.

E) Install POSNext on ERPNext (works in both dev + prod)

Now that Node 20 is set for both shell and Supervisor:
```
cd ~/frappe-bench
```

# cleanup if partial
```
rm -rf apps/pos_next
```
```
bench get-app https://github.com/BrainWise-DEV/POSNext
bench --site site1.local install-app pos_next
```
```
bench build
bench --site site1.local migrate
bench restart
```

F) “What updates/apps do I need?” (short list)
Must-have for POSNext

nvm
node v20.x
npm (bundled with node)
yarn 1.22.x (via corepack)
Already part of normal ERPNext server setup
git, curl
python3-dev, build-essential
mariadb
redis
supervisor

Quick “doctor” commands you can keep
```
node -v
npm -v
yarn -v
nvm current
bench version
bench --site site1.local list-apps | grep pos_next
```

If you paste the output of:
```
sudo ls -1 /etc/supervisor/conf.d
```

I can tell you the exact file name to edit on your server (since setups vary), but the steps above will work regardless.
