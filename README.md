# Fake-ransonware-in-python-
# Just a silly joke script that simulates a legit ransomware situation in python. Does not harm device or data. 
# To end the fake attack you simoly press escape and the message will "decrypt" data in 5 second and end the simulation. 
# if escape is not pressed amd timer runs out then the automation triggers a fake reboot with absolutly nothing happening to device and data. 
import tkinter as tk
import time
from threading import Thread, Event

class FakeRansomware:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("System Alert")
        self.root.attributes('-fullscreen', True)
        self.root.configure(bg='black')
        self.root.bind('<Escape>', self.deactivate)
        
        # Create main frame
        self.frame = tk.Frame(self.root, bg='black')
        self.frame.pack(expand=True)
        
        # Warning message
        self.warning = tk.Label(
            self.frame,
            text="YOUR SYSTEM HAS BEEN ENCRYPTED!",
            fg='red',
            bg='black',
            font=('Arial', 32, 'bold')
        )
        self.warning.pack(pady=30)
        
        # Instructions
        instructions = (
            "All your files have been encrypted with military-grade AES-256 encryption.\n\n"
            "To decrypt your files and prevent system destruction, you must pay 5 BTC within 12 hours.\n\n"
            "Press ESCAPE now to prove you're human and get decryption instructions."
        )
        self.instructions = tk.Label(
            self.frame,
            text=instructions,
            fg='white',
            bg='black',
            font=('Arial', 18),
            justify='left'
        )
        self.instructions.pack(pady=20)
        
        # Timer display
        self.timer_label = tk.Label(
            self.frame,
            text="12:00:00",
            fg='red',
            bg='black',
            font=('Courier New', 48, 'bold')
        )
        self.timer_label.pack(pady=40)
        
        # Disclaimer (small text at bottom)
        disclaimer = tk.Label(
            self.root,
            text="SIMULATION ONLY - NO FILES HAVE BEEN MODIFIED",
            fg='gray',
            bg='black',
            font=('Arial', 10)
        disclaimer.pack(side='bottom', pady=10)
        
        # Timer variables
        self.timer_running = Event()
        self.timer_running.set()
        self.countdown_seconds = 12 * 3600  # 12 hours
        
        # Start countdown in separate thread
        self.countdown_thread = Thread(target=self.update_timer)
        self.countdown_thread.daemon = True
        self.countdown_thread.start()
        
        # Center the window
        self.root.update_idletasks()
        self.root.mainloop()

    def update_timer(self):
        while self.countdown_seconds > 0 and self.timer_running.is_set():
            hours = self.countdown_seconds // 3600
            minutes = (self.countdown_seconds % 3600) // 60
            seconds = self.countdown_seconds % 60
            timer_text = f"{hours:02d}:{minutes:02d}:{seconds:02d}"
            self.timer_label.config(text=timer_text)
            time.sleep(1)
            self.countdown_seconds -= 1
        
        if self.countdown_seconds <= 0:
            self.initiate_fake_reboot()

    def deactivate(self, event=None):
        self.timer_running.clear()
        self.warning.config(text="DECRYPTION INITIATED!", fg='green')
        self.instructions.config(
            text="Your system is now being decrypted...\n\n"
                 "This simulation has been terminated.\n"
                 "Close this window to return to your normal system.",
            fg='lime'
        )
        self.timer_label.destroy()
        self.root.after(10000, self.root.destroy)  # Close after 10 seconds

    def initiate_fake_reboot(self):
        self.warning.config(text="SYSTEM DESTRUCTION INITIATED!")
        self.instructions.config(
            text="Critical system failure detected!\n\n"
                 "Simulating reboot sequence...\n"
                 "This window will close in 5 seconds.\n"
                 "NO ACTUAL CHANGES HAVE BEEN MADE TO YOUR SYSTEM.",
            fg='yellow'
        )
        self.root.update()
        time.sleep(5)
        self.root.destroy()

if __name__ == "__main__":
    print("Starting ransomware simulation... Press ESC in the GUI to 'deactivate'")
    app = FakeRansomware()
