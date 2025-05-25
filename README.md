/*
"use client";
import { useState } from "react";
import emailjs from "emailjs-com";

export default function Home() {
  const [formData, setFormData] = useState({
    from_name: "",
    from_email: "",
    preferred_datetime: "",
    message: "",
  });

  const [submitted, setSubmitted] = useState(false);
  const [sending, setSending] = useState(false);

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    setSending(true);

    try {
      await emailjs.send(
        process.env.NEXT_PUBLIC_EMAILJS_SERVICE_ID,
        process.env.NEXT_PUBLIC_EMAILJS_BOOKING_TEMPLATE_ID,
        formData,
        process.env.NEXT_PUBLIC_EMAILJS_PUBLIC_KEY
      );

      await emailjs.send(
        process.env.NEXT_PUBLIC_EMAILJS_SERVICE_ID,
        process.env.NEXT_PUBLIC_EMAILJS_THANK_YOU_TEMPLATE_ID,
        formData,
        process.env.NEXT_PUBLIC_EMAILJS_PUBLIC_KEY
      );

      setSubmitted(true);
      setFormData({ from_name: "", from_email: "", preferred_datetime: "", message: "" });
    } catch (err) {
      console.error(err);
      alert("Something went wrong. Please try again.");
    } finally {
      setSending(false);
    }
  };

  return (
    <main className="bg-black text-white min-h-screen font-sans">
      <section className="h-screen flex flex-col justify-center items-center text-center px-4">
        <h1 className="text-4xl font-bold mb-4">Lifestyle Companion</h1>
        <p className="text-lg text-gray-400 max-w-xl">Helping you book private, personalized lifestyle support sessions.</p>
        <a href="#booking" className="mt-8 px-6 py-3 bg-blue-600 rounded-full hover:bg-blue-500 transition-all">Book Now</a>
      </section>

      <section id="about" className="py-16 px-6 max-w-3xl mx-auto">
        <h2 className="text-2xl font-semibold mb-4">About</h2>
        <p className="text-gray-300">Lifestyle Companion offers a private and personalized approach to help you manage, improve, and simplify aspects of your life. Whether it’s emotional support, organizing your schedule, or planning something special — we’re here for you.</p>
      </section>

      <section id="booking" className="py-16 px-6 max-w-3xl mx-auto">
        <h2 className="text-2xl font-semibold mb-6">Book a Session</h2>
        {!submitted ? (
          <form onSubmit={handleSubmit} className="space-y-6 bg-gray-900 rounded-2xl p-8 shadow-lg">
            <input type="text" name="from_name" placeholder="Full Name" onChange={handleChange} value={formData.from_name} required className="w-full p-3 rounded bg-gray-800 border border-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" />
            <input type="email" name="from_email" placeholder="Email Address" onChange={handleChange} value={formData.from_email} required className="w-full p-3 rounded bg-gray-800 border border-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" />
            <input type="datetime-local" name="preferred_datetime" onChange={handleChange} value={formData.preferred_datetime} required className="w-full p-3 rounded bg-gray-800 border border-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" />
            <textarea name="message" rows="4" placeholder="Tell us what you need..." onChange={handleChange} value={formData.message} className="w-full p-3 rounded bg-gray-800 border border-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500"></textarea>
            <button type="submit" disabled={sending} className="w-full bg-blue-600 hover:bg-blue-500 transition rounded-full p-3 font-semibold">
              {sending ? "Sending..." : "Submit Booking Request"}
            </button>
          </form>
        ) : (
          <div className="bg-gray-900 rounded-2xl p-8 shadow-lg text-center">
            <h3 className="text-xl font-semibold mb-2">Thank you!</h3>
            <p>We’ve received your request and will respond shortly.</p>
          </div>
        )}
      </section>

      <footer className="bg-gray-900 py-8 px-6 text-center">
        <p className="mb-4">Connect with us:</p>
        <div className="flex justify-center space-x-4">
          <a href="https://instagram.com" target="_blank" rel="noopener noreferrer" className="hover:text-blue-400">Instagram</a>
          <a href="https://twitter.com" target="_blank" rel="noopener noreferrer" className="hover:text-blue-400">Twitter</a>
          <a href="https://t.me" target="_blank" rel="noopener noreferrer" className="hover:text-blue-400">Telegram</a>
        </div>
        <p className="mt-4 text-gray-500 text-sm">© {new Date().getFullYear()} Lifestyle Companion. All rights reserved.</p>
      </footer>
    </main>
  );
}
