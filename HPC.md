HPC Tips and Tricks

5) Aliases created for some of usual commands - Stored in .bashrc file. 
	
		5.1 To go to workind directory
			alias wdir='cd /hpfs/scratch/users/'
			alias wsdir = 'cd /hpfs/userws/'
		
		5.2 To see the contents of the dir.
			alias ll='ls -la'
	
		5.3 SLURM aliases
			
			5.3.1 
				alias getnode4='salloc --gres=gpu:2 --partition=gpu_short --time=04:00:00 srun --pty bash'
				alias sq='squeue'
		
			5.3.2 for cancelling the jobs. sc <JOBID>
				sc(){
					scancel "$@"
				}
	
			5.3.3 for getting different number gpus (1 to 4) on g004 for 4hrs
				getgpu4(){
					salloc --gres=gpu:$1 --partition=gpu_short --time=0$2:00:00 --mem=20G --mail-type=ALL srun --pty bash
				}
			
			5.3.4 for getting different number gpus (1 to 4) on other gpus g001-g003 for 4hrs
				getgpus(){
					salloc --gres=gpu:$1 --partition=gpu --time=0$2:00:00 --mem=20G --mail-type=ALL  srun --pty bash
				}
				# for getting different number gpus (1 to 4) on other gpus g001-g003 for the give time. takes three arguments. no of gpus, days and hours
				getgpus(){
					salloc --gres=gpu:$1 --partition=gpu --time=$2-$3:00:00 --mem=20G --mail-type=ALL  srun --pty bash
					}
				
		5.4 Creating sessions using tmux and other techniques
			5.4.1 ## Creating attaching killing tmux sessions
				
				tcreate(){
						tmux new -s $@
				}
	
				tattach(){
						tmux attach -t $@
				}
	
				tlist(){
						tmux list-sessions
				}
	
				tkill(){
						tmux kill-session -t $@
				}
		

6) Batch file format 

		#SBATCH --time 1:00:00  wall time of a job (or -t)
		#SBATCH --partition=name  partition to use (or -p)
		#SBATCH --account=name  account to use (or -A)
		#SBATCH --nodes=2  number of nodes (or -N)
		#SBATCH --ntasks 12  total number of tasks (or -n)
		#SBATCH --mail-type=FAIL,BEGIN,END  events on which to send email
		#SBATCH --mail-user=name@example.com  email address to use
		#SBATCH -o slurm-%j.out-%N  name for stdout; %j is job#, %N node
		#SBATCH -e slurm-%j.err-%N  name for stderr; %j is job#, %N node
		#SBATCH --constraint “C20”  can use features given for nodes (or –C)


8)	HPC commands :
		
		8.1 Loading/Unloading sub-commands:
			-------------------------------
			  load | add          module [...]  load module(s)
			  try-load | try-add  module [...]  Add module(s), do not complain if not found
			  del | unload        module [...]  Remove module(s), do not complain if not found
			  swap | sw | switch  m1 m2         unload m1 and load m2
			  purge                             unload all modules
			  refresh                           reload aliases from current list of modules.
			  update                            reload all currently loaded modules.

		8.2 Listing / Searching sub-commands:
			---------------------------------
			  list                              List loaded modules
			  list                s1 s2 ...     List loaded modules that match the pattern
			  avail | av                        List available modules
			  avail | av          string        List available modules that contain "string".
			  spider                            List all possible modules
			  spider              module        List all possible version of that module file
			  spider              string        List all module that contain the "string".
			  spider              name/version  Detailed information about that version of the module.
			  whatis              module        Print whatis information about module
			  keyword | key       string        Search all name and whatis that contain "string".

		8.3 Searching with Lmod:
			--------------------
			  All searching (spider, list, avail, keyword) support regular expressions:


			  -r spider           '^p'          Finds all the modules that start with `p' or `P'
			  -r spider           mpi           Finds all modules that have "mpi" in their name.
			  -r spider           'mpi$         Finds all modules that end with "mpi" in their name.
			  

9) Using multiple Terminals with Jupyter
	
    	9.1 commands 
    		create session - screen -S mysession
    						 salloc --time=10:00:00
    	
    	9.2 Then, you can detach from the session using Ctrl-A D. In your new terminal, 
    		you can reattach to the session with screen -r mysession.
    	
    	9.3 You can start the Jupyter notebook in the background by appending an ampersand (&) to your command. 
    		jupyter notebook &
    
    	9.4 This will start the Jupyter notebook in the background and you'll still have access to the terminal. 
    		You can then use Ctrl-A D to detach the screen session. When you want to check on your Jupyter notebook, 
    		you can reattach to the screen session with screen -r.
    		
    	9.5 Please keep in mind that running Jupyter notebook in the background like this means it won't automatically 
    		stop when you close the terminal. You'll need to stop it manually. You can do this by bringing the job to the 
    		foreground with the fg command and then using Ctrl-C to stop it.
    		
    		
    	9.6 Note down the pid when run 
    			e.g 391855
			  
			  
APPENDIX : 

1) 'Tmux' is a great tool for running and managing terminal sessions. Here's a basic guide on how to use it:

		1.1 To start a new session: tmux new -s mysession
		This command starts a new tmux session named "mysession".

		1.2 To detach from the session: tmux detach or you can press Ctrl-b followed by d.
		This command allows you to leave the session without terminating it.

		1.3 To list all sessions: tmux list-sessions
		This command lists all the current tmux sessions.

		1.4 To attach to an existing session: tmux attach -t mysession
		This command allows you to reattach to the "mysession".

		1.5 To kill a session: tmux kill-session -t mysession
		This command kills the "mysession".

		1.6 To split panes within a session:

			1.6.1 Horizontal split: Ctrl-b followed by %
			1.6.2 Vertical split: Ctrl-b followed by "
			1.6.3 To switch between panes: Ctrl-b followed by arrow keys

		1.7 To close a pane: Ctrl-d

	Please note that Ctrl-b is the default prefix key for tmux, which means you press this combination before entering 
	other tmux commands. You can customize this in your tmux configuration file.

	Remember, tmux sessions are persistent, which means they will stay active even if you get disconnected. 
	You can always reattach to them later. It's a great tool for running long jobs on remote servers.
