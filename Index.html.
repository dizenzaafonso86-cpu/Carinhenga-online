<!-- ====================================================================== -->
<!-- ======================= INÍCIO DE: index.html ======================== -->
<!-- ====================================================================== -->

<!DOCTYPE html>
<html lang="pt-PT">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Carinhenga Online</title>
    <link rel="manifest" href="/manifest.json" />
    <meta name="theme-color" content="#FBBF24" />
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
      body { font-family: 'Inter', sans-serif; }
      @keyframes zoom-in {
        from { opacity: 0; transform: scale(0.95); }
        to { opacity: 1; transform: scale(1); }
      }
      @keyframes fade-in {
        from { opacity: 0; }
        to { opacity: 1; }
      }
      @keyframes slide-in-from-bottom-2 {
        from { opacity: 0; transform: translateY(8px); }
        to { opacity: 1; transform: translateY(0); }
      }
      @keyframes slide-in-from-right-8 {
        from { opacity: 0; transform: translateX(32px); }
        to { opacity: 1; transform: translateX(0); }
      }
      .animate-in { animation: enter 0.2s ease-out; }
      .animate-in.fade-in { animation-name: fade-in; }
      .animate-in.zoom-in { animation-name: zoom-in; }
      .animate-in.slide-in-from-bottom-2 { animation-name: slide-in-from-bottom-2; }
      .animate-in.slide-in-from-right-8 { animation-name: slide-in-from-right-8; }
      .duration-300 { animation-duration: 300ms; }
      .duration-500 { animation-duration: 500ms; }
    </style>
  <script type="importmap">
{
  "imports": {
    "react": "https://esm.sh/react@19.0.0-rc.0",
    "react-dom/client": "https://esm.sh/react-dom@19.0.0-rc.0/client",
    "lucide-react": "https://esm.sh/lucide-react@0.378.0",
    "@google/genai": "https://esm.sh/@google/genai@0.6.0"
  }
}
</script>
</head>
  <body class="bg-gray-50 text-gray-900">
    <div id="root"></div>
    <script type="module">
      import React, { useState, useEffect, useCallback, useRef } from 'react';
      import ReactDOM from 'react-dom/client';
      import { 
        ShoppingBag, MessageSquare, PlusCircle, User as UserIcon, Search, Bell, 
        DollarSign, TrendingUp, LogOut, ChevronRight, Send, Camera, CheckCircle,
        X, Check, CheckCheck, RefreshCw, CreditCard, Banknote, ShieldCheck, Phone,
        Calendar, AlertCircle, MapPin, Map, Users, Edit3, Save, Tag, Share2, 
        ExternalLink, Handshake, Heart, Package, ShoppingCart, ArrowLeft, FileText,
        Upload, Star, Sparkles, Loader2, Shield, Key, Mail, UserPlus, HelpCircle,
        Filter, EyeOff, Eye
      } from 'lucide-react';
      import { GoogleGenAI, Type } from "@google/genai";

      // --- INÍCIO: Conteúdo de constantes, tipos e serviços ---
      const BANK_COORDINATES = {
        bank: "Banco Millenium Atlântico",
        accountName: "Carinhenga Online Admin",
        iban: "0006 0000 5781 5296 3012 0",
        express: "921463001"
      };

      const PRICING = {
        monthly: 200,
        annual: 3000,
        currency: "Kz"
      };

      const CATEGORIES = [
        "Eletrônicos", "Vestuário", "Alimentos", "Casa e Jardim", 
        "Automóveis", "Serviços", "Outros"
      ];
      
      const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });

      const getProductDescriptionSuggestion = async (title, category) => {
        try {
          const response = await ai.models.generateContent({
            model: 'gemini-3-flash-preview',
            contents: `Sugira uma descrição de venda atraente em português para um produto chamado "${title}" na categoria "${category}". Seja breve e persuasivo.`,
          });
          return response.text;
        } catch (error) {
          console.error("Gemini Error:", error);
          return "";
        }
      };

      const getSimilarProductsRecommendation = async (targetProduct, candidates) => {
        try {
          const response = await ai.models.generateContent({
            model: 'gemini-3-flash-preview',
            contents: `Analise o produto alvo: "${targetProduct.title}" (Categoria: ${targetProduct.category}, Preço: ${targetProduct.price} Kz).
            Dentre os seguintes candidatos, escolha os 3 mais similares com base na categoria e proximidade de preço. 
            Retorne apenas um array JSON com os IDs dos produtos escolhidos.
            Candidatos:
            ${candidates.map(c => `ID: ${c.id}, Título: ${c.title}, Categoria: ${c.category}, Preço: ${c.price}`).join('\n')}`,
            config: {
              responseMimeType: "application/json",
              responseSchema: {
                type: Type.ARRAY,
                items: { type: Type.STRING }
              }
            }
          });
          return JSON.parse(response.text || "[]");
        } catch (error) {
          console.error("Gemini Recommendations Error:", error);
          return [];
        }
      };
      // --- FIM: Conteúdo de constantes, tipos e serviços ---


      // --- INÍCIO: Componente Principal da Aplicação (App.tsx) ---
      const SubscriptionWall = ({ onPaid, onClose }) => {
        const [selectedPlan, setSelectedPlan] = useState('Mensal');
        const [proof, setProof] = useState(null);
        const [error, setError] = useState(null);

        const handleFileChange = (e) => {
          const file = e.target.files?.[0];
          if (file) {
            setError(null);
            const reader = new FileReader();
            reader.onloadend = () => setProof(reader.result);
            reader.readAsDataURL(file);
          }
        };

        const handleConfirm = () => {
          if (!proof) {
            setError("Por favor, anexe o comprovativo de pagamento para continuar.");
            return;
          }
          onPaid(selectedPlan, proof);
        };

        return (
          React.createElement("div", { className: "fixed inset-0 bg-black/80 flex items-center justify-center z-[100] p-4 backdrop-blur-md" },
            React.createElement("div", { className: "bg-white rounded-[40px] p-8 max-w-lg w-full shadow-2xl overflow-y-auto max-h-[95vh] animate-in fade-in zoom-in duration-300 relative" },
              React.createElement("div", { className: "flex justify-between items-center mb-6" },
                React.createElement("div", { className: "bg-amber-100 w-12 h-12 rounded-2xl flex items-center justify-center" }, React.createElement(ShieldCheck, { className: "text-amber-600 w-6 h-6" })),
                React.createElement("button", { onClick: onClose, className: "p-2 hover:bg-gray-100 rounded-full transition-colors" }, React.createElement(X, { className: "w-6 h-6 text-gray-400" }))
              ),
              React.createElement("h2", { className: "text-3xl font-black text-gray-900 mb-2 tracking-tighter" }, "Acesso Premium"),
              React.createElement("p", { className: "text-gray-500 font-medium mb-8" }, "Você atingiu o limite de 2 vendas gratuitas. A Taxa de Manutenção é necessária para continuar anunciando e realizando compras."),
              React.createElement("div", { className: "grid grid-cols-2 gap-4 mb-8" },
                React.createElement("div", { onClick: () => setSelectedPlan('Mensal'), className: `border-4 rounded-3xl p-5 transition-all cursor-pointer relative overflow-hidden ${selectedPlan === 'Mensal' ? 'border-amber-500 bg-amber-50' : 'border-gray-100 bg-gray-50 hover:border-gray-200'}` },
                  selectedPlan === 'Mensal' && React.createElement(CheckCircle, { className: "absolute top-3 right-3 text-amber-600 w-5 h-5" }),
                  React.createElement("p", { className: "font-black text-2xl text-gray-900" }, PRICING.monthly, PRICING.currency),
                  React.createElement("p", { className: "text-xs text-gray-500 font-bold uppercase tracking-widest mt-1" }, "Mensal")
                ),
                React.createElement("div", { onClick: () => setSelectedPlan('Anual'), className: `border-4 rounded-3xl p-5 transition-all cursor-pointer relative overflow-hidden ${selectedPlan === 'Anual' ? 'border-amber-500 bg-amber-50' : 'border-gray-100 bg-gray-50 hover:border-gray-200'}` },
                  selectedPlan === 'Anual' && React.createElement(CheckCircle, { className: "absolute top-3 right-3 text-amber-600 w-5 h-5" }),
                  React.createElement("p", { className: "font-black text-2xl text-gray-900" }, PRICING.annual, PRICING.currency),
                  React.createElement("p", { className: "text-xs text-gray-500 font-bold uppercase tracking-widest mt-1" }, "Anual")
                )
              ),
              React.createElement("div", { className: "bg-blue-50 p-6 rounded-3xl mb-8 border-2 border-blue-100" },
                React.createElement("p", { className: "font-black text-blue-800 mb-4 flex items-center text-sm" }, React.createElement(CreditCard, { className: "w-4 h-4 mr-2" }), " Coordenadas Bancárias"),
                React.createElement("div", { className: "space-y-4 font-bold text-blue-900 text-sm" },
                  React.createElement("div", { className: "flex flex-col" },
                    React.createElement("span", { className: "text-[10px] text-blue-600 uppercase mb-1" }, "IBAN"),
                    React.createElement("span", { className: "font-mono text-xs select-all bg-white p-2 rounded-xl border border-blue-200" }, BANK_COORDINATES.iban)
                  ),
                  React.createElement("div", { className: "flex justify-between items-center" },
                    React.createElement("span", { className: "text-[10px] text-blue-600 uppercase" }, "Express (WhatsApp para Comprovativos)"),
                    React.createElement("span", { className: "bg-white px-3 py-1 rounded-lg border border-blue-200" }, BANK_COORDINATES.express)
                  )
                )
              ),
              React.createElement("div", { className: "mb-8" },
                React.createElement("p", { className: "text-sm font-black text-gray-700 mb-3 ml-1 uppercase tracking-widest flex items-center" }, React.createElement(FileText, { className: "w-4 h-4 mr-2" }), " Anexar Comprovativo"),
                proof ? (
                  React.createElement("div", { className: "relative aspect-video rounded-3xl overflow-hidden border-4 border-amber-100 bg-gray-100" },
                    React.createElement("img", { src: proof, alt: "Comprovativo", className: "w-full h-full object-cover" }),
                    React.createElement("button", { onClick: () => setProof(null), className: "absolute top-2 right-2 bg-white p-2 rounded-full shadow-lg text-red-500 hover:scale-110 transition-transform" }, React.createElement(X, { className: "w-4 h-4" }))
                  )
                ) : (
                  React.createElement("label", { className: `flex flex-col items-center justify-center p-8 border-4 border-dashed rounded-[32px] text-center bg-gray-50 hover:bg-amber-50 hover:border-amber-200 transition-all cursor-pointer group ${error ? 'border-red-300' : 'border-gray-200'}` },
                    React.createElement(Upload, { className: "w-8 h-8 text-amber-500 mb-3 group-hover:scale-110 transition-transform" }),
                    React.createElement("p", { className: "text-sm font-black text-gray-700 uppercase tracking-tighter" }, "Escolher Arquivo"),
                    React.createElement("input", { type: "file", accept: "image/*,application/pdf", onChange: handleFileChange, className: "hidden" })
                  )
                )
              ),
              error && (
                React.createElement("div", { className: "bg-red-50 border border-red-200 text-red-700 px-4 py-3 rounded-2xl mb-6 text-sm font-bold flex items-center gap-2 animate-in fade-in" },
                  React.createElement(AlertCircle, { className: "w-5 h-5" }),
                  error
                )
              ),
              React.createElement("button", { onClick: handleConfirm, className: "w-full bg-amber-600 text-white py-5 rounded-[24px] font-black text-lg hover:bg-amber-700 transition-all shadow-xl shadow-amber-200" }, "Confirmar Pagamento Realizado")
            )
          )
        );
      };

      const ChatWindow = ({ chat, currentUser, messages, products, onSendMessage }) => {
        const [text, setText] = useState("");
        const messagesEndRef = useRef(null);
        const product = products.find(p => p.id === chat.productId);

        const scrollToBottom = () => {
          messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
        };

        useEffect(() => {
          scrollToBottom();
        }, [messages]);

        const handleSend = () => {
          if (!text.trim()) return;
          onSendMessage(text);
          setText("");
        };

        const renderStatusIcon = (status, isMe) => {
          if (!isMe) return null;
          switch (status) {
            case 'sent': return React.createElement(Check, { className: "w-3 h-3 text-white/70" });
            case 'delivered': return React.createElement(CheckCheck, { className: "w-3 h-3 text-white/70" });
            case 'read': return React.createElement(CheckCheck, { className: "w-3 h-3 text-sky-200" });
            default: return null;
          }
        };

        return (
          React.createElement("div", { className: "flex flex-col h-[600px] bg-white rounded-[40px] shadow-2xl overflow-hidden border border-gray-100" },
            React.createElement("div", { className: `p-6 border-b flex justify-between items-center ${chat.intermediaryId ? 'bg-indigo-50' : 'bg-amber-50'}` },
              React.createElement("div", { className: "flex items-center space-x-4" },
                React.createElement("div", { className: `w-12 h-12 rounded-2xl flex items-center justify-center shadow-sm ${chat.intermediaryId ? 'bg-indigo-600 text-white' : 'bg-amber-600 text-white'}` },
                  chat.intermediaryId ? React.createElement(Handshake, { className: "w-6 h-6" }) : React.createElement(MessageSquare, { className: "w-6 h-6" })
                ),
                React.createElement("div", null,
                  React.createElement("h3", { className: "font-black text-gray-900 leading-tight" }, chat.intermediaryId ? 'Negociação Assistida' : 'Negociação Direta'),
                  React.createElement("p", { className: "text-[10px] font-black uppercase tracking-widest text-gray-500" }, product?.title || 'Produto Indisponível')
                )
              ),
              chat.intermediaryId && (
                React.createElement("div", { className: "bg-indigo-100 text-indigo-700 px-3 py-1 rounded-full text-[10px] font-black uppercase flex items-center" },
                  React.createElement(Shield, { className: "w-3 h-3 mr-1" }), " Intermediado"
                )
              )
            ),
            React.createElement("div", { className: "flex-1 overflow-y-auto p-6 space-y-6 bg-gray-50/50" },
              messages.length === 0 && (
                React.createElement("div", { className: "h-full flex flex-col items-center justify-center text-gray-400 space-y-2 opacity-60" },
                  React.createElement(MessageSquare, { className: "w-12 h-12" }),
                  React.createElement("p", { className: "font-bold text-sm" }, "Inicie a conversa!")
                )
              ),
              messages.map((m) => {
                const isMe = m.senderId === currentUser.id;
                const roleLabel = m.senderId === chat.intermediaryId ? "Intermediador" : (product && m.senderId === product.sellerId ? "Vendedor" : "Comprador");
                return (
                  React.createElement("div", { key: m.id, className: `flex ${isMe ? 'justify-end' : 'justify-start'} animate-in slide-in-from-bottom-2 duration-300` },
                    React.createElement("div", { className: `max-w-[80%] flex flex-col ${isMe ? 'items-end' : 'items-start'}` },
                      React.createElement("span", { className: "text-[9px] font-black uppercase tracking-widest text-gray-400 mb-1 px-2" }, `${roleLabel} ${isMe ? '• Você' : ''}`),
                      React.createElement("div", { className: `px-4 py-3 rounded-3xl shadow-sm relative ${isMe ? (chat.intermediaryId ? 'bg-indigo-600 text-white rounded-br-none' : 'bg-amber-600 text-white rounded-br-none') : 'bg-white text-gray-800 border border-gray-100 rounded-bl-none'}` },
                        React.createElement("p", { className: "text-sm font-medium leading-relaxed" }, m.text),
                        React.createElement("div", { className: "flex items-center justify-end space-x-1 mt-1 opacity-80" },
                          React.createElement("span", { className: "text-[8px] font-bold" }, new Date(m.timestamp).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })),
                          renderStatusIcon(m.status, isMe)
                        )
                      )
                    )
                  )
                );
              }),
              React.createElement("div", { ref: messagesEndRef })
            ),
            React.createElement("div", { className: "p-6 border-t bg-white flex items-center space-x-3" },
              React.createElement("input", {
                type: "text", value: text, onChange: (e) => setText(e.target.value),
                onKeyPress: (e) => e.key === 'Enter' && handleSend(),
                placeholder: "Sua proposta...",
                className: "flex-1 border-2 border-gray-100 rounded-2xl px-6 py-4 focus:ring-4 focus:ring-amber-500/10 focus:border-amber-600 outline-none transition-all font-medium text-sm"
              }),
              React.createElement("button", { onClick: handleSend, className: `p-4 rounded-2xl text-white shadow-lg active:scale-95 transition-all ${chat.intermediaryId ? 'bg-indigo-600' : 'bg-amber-600'}` },
                React.createElement(Send, { className: "w-6 h-6" })
              )
            )
          )
        );
      };

      const Footer = () => (
        React.createElement("footer", { className: "bg-white border-t mt-12 py-10" },
          React.createElement("div", { className: "max-w-7xl mx-auto px-6 text-center text-sm text-gray-500" },
            React.createElement("div", { className: "flex items-center justify-center space-x-2 mb-5" },
              React.createElement("div", { className: "bg-amber-600 p-1.5 rounded-lg" },
                React.createElement(ShoppingBag, { className: "h-5 w-5 text-white" })
              ),
              React.createElement("span", { className: "text-lg font-black text-gray-900 tracking-tighter" }, "Carinhenga", React.createElement("span", { className: "text-amber-600" }, "Online"))
            ),
            React.createElement("div", { className: "flex justify-center gap-x-6 gap-y-2 flex-wrap mb-5" },
              React.createElement("a", { href: "#", className: "font-bold hover:text-amber-600 transition" }, "Sobre Nós"),
              React.createElement("a", { href: "#", className: "font-bold hover:text-amber-600 transition" }, "Termos de Serviço"),
              React.createElement("a", { href: "#", className: "font-bold hover:text-amber-600 transition" }, "Política de Privacidade"),
              React.createElement("a", { href: "#", className: "font-bold hover:text-amber-600 transition" }, "Ajuda")
            ),
            React.createElement("p", { className: "text-xs" }, `© ${new Date().getFullYear()} Carinhenga Online. Todos os direitos reservados.`)
          )
        )
      );

      const DUMMY_USER_CREDENTIALS = {
        email: 'user@carinhenga.com',
        password: 'password123',
        user: {
            id: 'u_dummy',
            name: 'Utilizador Fictício',
            username: 'dummyuser',
            email: 'user@carinhenga.com',
            salesCount: 5,
            purchaseCount: 2,
            hasPaidSubscription: false,
            bankDetailsSeen: false,
            favoriteProductIds: ['1'],
            purchasedProductIds: [],
            phone: '923123456'
        }
      };

      function App() {
        const [currentUser, setCurrentUser] = useState(null);
        const [view, setView] = useState('home'); 
        const [authMode, setAuthMode] = useState('login');
        const [profileSubView, setProfileSubView] = useState('info');
        const [products, setProducts] = useState([]);
        const [wantedProducts, setWantedProducts] = useState([]);
        const [selectedProduct, setSelectedProduct] = useState(null);
        const [searchTerm, setSearchTerm] = useState("");
        const [isEditingProfile, setIsEditingProfile] = useState(false);
        const [showSubscriptionWall, setShowSubscriptionWall] = useState(false);
        const [notifications, setNotifications] = useState([]);
        const [showNotification, setShowNotification] = useState(false);
        const [chats, setChats] = useState([]);
        const [activeChatId, setActiveChatId] = useState(null);
        const [messages, setMessages] = useState([]);
        const [homeTab, setHomeTab] = useState('venda');
        const [authError, setAuthError] = useState(null);
        const [isSuggestingDescription, setIsSuggestingDescription] = useState(false);
        const [newProductDescription, setNewProductDescription] = useState('');
        const [recommendedProducts, setRecommendedProducts] = useState([]);
        const [isLoadingRecs, setIsLoadingRecs] = useState(false);

        useEffect(() => {
          const storedUser = localStorage.getItem('carinhenga_user');
          if (storedUser) setCurrentUser(JSON.parse(storedUser));
          setProducts([
            { id: '1', sellerId: 'u1', sellerName: 'João Silva', sellerSalesCount: 1, title: 'iPhone 13 Pro', price: 450000, description: 'Estado novo, 256GB bateria 100%.', category: 'Eletrônicos', imageUrl: 'https://images.unsplash.com/photo-1632661674596-df8be070a5c5?auto=format&fit=crop&q=80&w=400', createdAt: Date.now(), isSold: false },
            { id: '2', sellerId: 'u2', sellerName: 'Maria Pedro', sellerSalesCount: 0, title: 'Ténis Nike Air Force 1', price: 25000, description: 'Tamanho 42, original.', category: 'Vestuário', imageUrl: 'https://images.unsplash.com/photo-1595950653106-6c9ebd614d3a?auto=format&fit=crop&q=80&w=400', createdAt: Date.now(), isSold: false },
            { id: '3', sellerId: 'u1', sellerName: 'João Silva', sellerSalesCount: 1, title: 'Macbook Pro 14"', price: 1200000, description: 'Processador M1 Pro, 16GB RAM, 512GB SSD.', category: 'Eletrônicos', imageUrl: 'https://images.unsplash.com/photo-1634430224210-95373a824db5?auto=format&fit=crop&q=80&w=400', createdAt: Date.now(), isSold: false },
          ]);
        }, []);

        useEffect(() => {
          const fetchRecommendations = async () => {
            if (view === 'product-details' && selectedProduct) {
              setIsLoadingRecs(true);
              setRecommendedProducts([]);
              const otherProducts = products.filter(p => p.id !== selectedProduct.id && !p.isSold);
              try {
                const recIds = await getSimilarProductsRecommendation(selectedProduct, otherProducts);
                const recs = products.filter(p => recIds.includes(p.id));
                setRecommendedProducts(recs);
              } catch (error) {
                console.error("Failed to fetch recommendations:", error);
              } finally {
                setIsLoadingRecs(false);
              }
            }
          };
          fetchRecommendations();
        }, [selectedProduct, view, products]);

        const validateEmail = (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);

        const handleAuth = (e) => {
          e.preventDefault();
          setAuthError(null);
          const formData = new FormData(e.currentTarget);
          const email = formData.get('email');
          const password = formData.get('password');

          if (!validateEmail(email)) {
            setAuthError("Por favor, insira um formato de e-mail válido.");
            return;
          }

          if (authMode === 'login') {
            if (email === DUMMY_USER_CREDENTIALS.email && password === DUMMY_USER_CREDENTIALS.password) {
              setCurrentUser(DUMMY_USER_CREDENTIALS.user);
              localStorage.setItem('carinhenga_user', JSON.stringify(DUMMY_USER_CREDENTIALS.user));
              setView('home');
            } else {
              setAuthError('E-mail ou palavra-passe incorretos.');
            }
            return;
          }

          if (authMode === 'forgot') {
            alert("Link de recuperação enviado.");
            setAuthMode('login');
            return;
          }
          
          const newUser = {
            id: Math.random().toString(36).substr(2, 9),
            name: formData.get('name') || email.split('@')[0],
            username: formData.get('username') || email.split('@')[0],
            email, salesCount: 0, purchaseCount: 0, hasPaidSubscription: false,
            bankDetailsSeen: false, favoriteProductIds: [], purchasedProductIds: []
          };
          setCurrentUser(newUser);
          localStorage.setItem('carinhenga_user', JSON.stringify(newUser));
          setView('home');
        };

        const handleSaveProfile = (e) => {
          e.preventDefault();
          if (!currentUser) return;
          const formData = new FormData(e.currentTarget);
          const updatedUser = {
            ...currentUser,
            name: formData.get('name'),
            username: formData.get('username'),
            phone: formData.get('phone'),
          };
          setCurrentUser(updatedUser);
          localStorage.setItem('carinhenga_user', JSON.stringify(updatedUser));
          setIsEditingProfile(false);
          triggerNotify("Perfil atualizado com sucesso!");
        };

        const triggerNotify = (text) => {
          setNotifications([text, ...notifications]);
          setShowNotification(true);
          setTimeout(() => setShowNotification(false), 4000);
        };

        const handleAddProduct = (e) => {
          e.preventDefault();
          if (!currentUser) return setView('auth');
          if (currentUser.salesCount >= 2 && !currentUser.hasPaidSubscription) return setShowSubscriptionWall(true);
          const formData = new FormData(e.currentTarget);
          const title = formData.get('title');
          const newProduct = {
            id: Math.random().toString(36).substr(2, 9),
            sellerId: currentUser.id,
            sellerName: currentUser.name,
            sellerSalesCount: currentUser.salesCount,
            title,
            price: Number(formData.get('price')),
            description: newProductDescription,
            category: formData.get('category'),
            imageUrl: `https://picsum.photos/400/300?${Math.random()}`,
            createdAt: Date.now(),
            isSold: false
          };
          setProducts([newProduct, ...products]);
          const updatedUser = { ...currentUser, salesCount: currentUser.salesCount + 1 };
          setCurrentUser(updatedUser);
          localStorage.setItem('carinhenga_user', JSON.stringify(updatedUser));
          setView('home');
          setNewProductDescription('');
          const matches = wantedProducts.filter(w => w.title.toLowerCase().includes(title.toLowerCase()) || title.toLowerCase().includes(w.title.toLowerCase()));
          if (matches.length > 0) {
            triggerNotify(`Opa! ${matches.length} pessoas estão procurando por este produto!`);
          }
        };

        const handleSuggestDescription = async (e) => {
          const form = e.currentTarget.form;
          if (!form) return;
          const title = form.elements.namedItem('title')?.value;
          const category = form.elements.namedItem('category')?.value;
          if (!title || !category) {
            triggerNotify("Por favor, preencha o título e a categoria primeiro.");
            return;
          }
          setIsSuggestingDescription(true);
          try {
            const suggestion = await getProductDescriptionSuggestion(title, category);
            if (suggestion) setNewProductDescription(suggestion);
          } catch (error) {
            console.error("Failed to get description suggestion:", error);
            triggerNotify("Não foi possível gerar uma sugestão.");
          } finally {
            setIsSuggestingDescription(false);
          }
        };

        const toggleSoldStatus = (productId) => {
          setProducts(products.map(p => p.id === productId ? { ...p, isSold: !p.isSold } : p));
          if (selectedProduct && selectedProduct.id === productId) {
              setSelectedProduct({...selectedProduct, isSold: !selectedProduct.isSold});
          }
          const product = products.find(p => p.id === productId);
          triggerNotify(product?.isSold ? "Produto marcado como disponível!" : "Produto marcado como vendido!");
        };

        const handleAddWanted = (e) => {
          e.preventDefault();
          if (!currentUser) return setView('auth');
          const formData = new FormData(e.currentTarget);
          const newWanted = {
            id: Math.random().toString(36).substr(2, 9),
            userId: currentUser.id,
            userName: currentUser.name,
            title: formData.get('title'),
            category: formData.get('category'),
            description: formData.get('description'),
            createdAt: Date.now()
          };
          setWantedProducts([newWanted, ...wantedProducts]);
          setView('home');
          setHomeTab('procura');
          triggerNotify("Seu pedido de 'Procura-se' foi publicado!");
        };

        const startChat = (receiverId, receiverName, productId) => {
          if (!currentUser) return setView('auth');
          if (currentUser.id === receiverId) return;
          const newChat = { id: Math.random().toString(36).substr(2, 9), participants: [currentUser.id, receiverId], productId };
          setChats([newChat, ...chats]);
          setActiveChatId(newChat.id);
          setView('chats');
        };

        const ProductCard = ({ product, isOwner }) => (
          React.createElement("div", { onClick: () => { setSelectedProduct(product); setView('product-details'); }, className: `bg-white rounded-[32px] shadow-sm border border-gray-100 overflow-hidden hover:shadow-xl transition-all cursor-pointer group flex flex-col h-full relative ${product.isSold ? 'opacity-75' : ''}` },
            React.createElement("div", { className: "h-48 bg-gray-50 overflow-hidden shrink-0 relative" },
              React.createElement("img", { src: product.imageUrl, alt: product.title, className: `w-full h-full object-cover group-hover:scale-105 transition-transform duration-500 ${product.isSold ? 'grayscale' : ''}` }),
              product.isSold && React.createElement("div", { className: "absolute inset-0 bg-black/40 flex items-center justify-center" },
                React.createElement("span", { className: "bg-red-600 text-white px-4 py-2 rounded-xl font-black text-xs uppercase tracking-widest shadow-lg" }, "Vendido")
              )
            ),
            React.createElement("div", { className: "p-4 flex flex-col flex-1" },
              React.createElement("div", { className: "flex justify-between items-start mb-1" },
                React.createElement("h3", { className: `font-black text-gray-900 leading-tight truncate ${product.isSold ? 'line-through decoration-red-500' : ''}` }, product.title),
                React.createElement("span", { className: "text-amber-600 font-black text-xs whitespace-nowrap ml-2" }, `${product.price.toLocaleString()} Kz`)
              ),
              React.createElement("p", { className: "text-gray-400 text-[10px] font-bold uppercase tracking-widest" }, product.category),
              React.createElement("div", { className: "mt-auto pt-3 flex gap-2" },
                isOwner ? (
                  React.createElement("button", { onClick: (e) => { e.stopPropagation(); toggleSoldStatus(product.id); }, className: `flex-1 font-black py-2 rounded-xl text-[9px] transition-all flex items-center justify-center gap-1 ${product.isSold ? 'bg-green-50 text-green-600' : 'bg-red-50 text-red-600'}` },
                    product.isSold ? React.createElement(React.Fragment, null, React.createElement(Eye, { className: "w-3 h-3" }), " Reativar") : React.createElement(React.Fragment, null, React.createElement(EyeOff, { className: "w-3 h-3" }), " Marcar Vendido")
                  )
                ) : (
                  !product.isSold && React.createElement("button", { onClick: (e) => { e.stopPropagation(); startChat(product.sellerId, product.sellerName, product.id); }, className: "flex-1 bg-amber-50 text-amber-600 font-black py-2 rounded-xl text-[9px] hover:bg-amber-100 transition-all flex items-center justify-center gap-1" },
                    React.createElement(MessageSquare, { className: "w-3 h-3" }), " Negociar"
                  )
                )
              )
            )
          )
        );

        return (
          React.createElement("div", { className: "min-h-screen bg-[#FDFCF9] pb-24 md:pb-0 flex flex-col" },
            React.createElement("div", { className: "flex-grow" },
              React.createElement("nav", { className: "bg-white border-b sticky top-0 z-40 px-6 h-16 flex items-center justify-between shadow-sm" },
                React.createElement("div", { className: "flex items-center space-x-2 cursor-pointer", onClick: () => setView('home') },
                  React.createElement("div", { className: "bg-amber-600 p-1.5 rounded-lg" }, React.createElement(ShoppingBag, { className: "h-6 w-6 text-white" })),
                  React.createElement("span", { className: "text-xl font-black text-gray-900 tracking-tighter" }, "Carinhenga", React.createElement("span", { className: "text-amber-600" }, "Online"))
                ),
                React.createElement("div", { className: "flex items-center space-x-4" },
                  currentUser ? (
                    React.createElement("button", { onClick: () => { setView('profile'); setProfileSubView('info'); }, className: "w-10 h-10 rounded-full bg-amber-100 border-2 border-white shadow-sm flex items-center justify-center text-amber-700 font-bold overflow-hidden" },
                      currentUser.avatarUrl ? React.createElement("img", { src: currentUser.avatarUrl, className: "w-full h-full object-cover" }) : currentUser.name[0].toUpperCase()
                    )
                  ) : (
                    React.createElement("button", { onClick: () => { setView('auth'); setAuthMode('login'); }, className: "text-amber-600 font-black text-sm uppercase" }, "Entrar")
                  )
                )
              ),
              showNotification && React.createElement("div", { className: "fixed top-20 right-6 z-50 bg-amber-600 text-white px-6 py-4 rounded-3xl shadow-2xl flex items-center space-x-4 animate-in slide-in-from-right-8 duration-500" },
                React.createElement(Bell, { className: "w-6 h-6 animate-bounce" }),
                React.createElement("p", { className: "font-black text-sm" }, notifications[0]),
                React.createElement("button", { onClick: () => setShowNotification(false) }, React.createElement(X, { className: "w-5 h-5" }))
              ),
              showSubscriptionWall && React.createElement(SubscriptionWall, { onPaid: (plan, proof) => { setCurrentUser({...currentUser, hasPaidSubscription: true, subscriptionType: plan, paymentProofUrl: proof }); setShowSubscriptionWall(false); triggerNotify("Pagamento recebido! Bem-vindo ao Premium."); }, onClose: () => setShowSubscriptionWall(false) }),
              React.createElement("main", { className: "max-w-7xl mx-auto p-6" },
                view === 'home' && React.createElement("div", { className: "space-y-8" },
                  React.createElement("div", { className: "flex flex-col md:flex-row md:items-end justify-between gap-6" },
                    React.createElement("div", { className: "flex gap-4 p-1 bg-gray-100 rounded-3xl w-fit shrink-0" },
                      React.createElement("button", { onClick: () => setHomeTab('venda'), className: `px-6 py-2.5 rounded-[20px] font-black text-xs uppercase tracking-widest transition-all ${homeTab === 'venda' ? 'bg-white shadow-sm text-amber-600' : 'text-gray-400'}` }, "Produtos à Venda"),
                      React.createElement("button", { onClick: () => setHomeTab('procura'), className: `px-6 py-2.5 rounded-[20px] font-black text-xs uppercase tracking-widest transition-all ${homeTab === 'procura' ? 'bg-white shadow-sm text-amber-600' : 'text-gray-400'}` }, "Procura-se")
                    ),
                    React.createElement("div", { className: "flex-1 max-w-lg relative" },
                      React.createElement(Search, { className: "absolute left-4 top-1/2 -translate-y-1/2 text-gray-300 w-5 h-5" }),
                      React.createElement("input", { type: "text", placeholder: "Pesquisar...", onChange: (e) => setSearchTerm(e.target.value), className: "w-full pl-12 pr-6 py-4 bg-white border border-gray-100 rounded-2xl focus:ring-2 focus:ring-amber-500 outline-none shadow-sm font-bold text-sm" })
                    ),
                    React.createElement("button", { onClick: () => setView(homeTab === 'venda' ? 'add-product' : 'add-wanted'), className: "bg-amber-600 text-white px-8 py-4 rounded-2xl font-black hover:bg-amber-700 transition shadow-lg shadow-amber-100 flex items-center shrink-0 uppercase tracking-tighter text-sm" },
                      React.createElement(PlusCircle, { className: "w-5 h-5 mr-2" }), ` ${homeTab === 'venda' ? 'Anunciar Venda' : 'Pedir Produto'}`
                    )
                  ),
                  homeTab === 'venda' ? (
                    React.createElement("div", { className: "grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6" },
                      products.filter(p => p.title.toLowerCase().includes(searchTerm.toLowerCase())).map(p => React.createElement(ProductCard, { key: p.id, product: p, isOwner: currentUser?.id === p.sellerId }))
                    )
                  ) : (
                    React.createElement("div", { className: "grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" },
                      wantedProducts.filter(w => w.title.toLowerCase().includes(searchTerm.toLowerCase())).map(wanted => (
                        React.createElement("div", { key: wanted.id, className: "bg-white p-6 rounded-[32px] border border-gray-100 shadow-sm flex flex-col animate-in fade-in" },
                          React.createElement("div", { className: "flex justify-between items-start mb-4" },
                            React.createElement("div", { className: "bg-amber-100 text-amber-600 p-3 rounded-2xl" }, React.createElement(Search, { className: "w-6 h-6" })),
                            React.createElement("span", { className: "text-[10px] font-black bg-gray-100 text-gray-500 px-3 py-1 rounded-full uppercase" }, wanted.category)
                          ),
                          React.createElement("h3", { className: "font-black text-xl text-gray-900 mb-2" }, wanted.title),
                          React.createElement("p", { className: "text-gray-500 text-sm font-medium leading-relaxed mb-6" }, wanted.description),
                          React.createElement("div", { className: "mt-auto flex items-center justify-between border-t pt-4" },
                            React.createElement("div", { className: "flex items-center space-x-3" },
                              React.createElement("div", { className: "w-8 h-8 rounded-full bg-amber-600 flex items-center justify-center text-white text-xs font-black" }, wanted.userName[0]),
                              React.createElement("span", { className: "text-xs font-bold text-gray-700" }, wanted.userName)
                            ),
                            React.createElement("button", { onClick: () => startChat(wanted.userId, wanted.userName), className: "text-amber-600 font-black text-xs uppercase hover:underline" }, "Eu tenho este!")
                          )
                        )
                      ))
                    )
                  )
                ),
                view === 'product-details' && selectedProduct && React.createElement("div", { className: "max-w-4xl mx-auto space-y-10 animate-in fade-in" },
                  React.createElement("button", { onClick: () => setView('home'), className: "flex items-center text-amber-600 font-black" }, React.createElement(ArrowLeft, { className: "w-5 h-5 mr-2" }), " Voltar"),
                  React.createElement("div", { className: "grid grid-cols-1 md:grid-cols-2 gap-12" },
                    React.createElement("div", { className: "aspect-square bg-white rounded-[40px] overflow-hidden shadow-2xl border-4 border-white relative" },
                      React.createElement("img", { src: selectedProduct.imageUrl, className: `w-full h-full object-cover ${selectedProduct.isSold ? 'grayscale' : ''}` }),
                      selectedProduct.isSold && React.createElement("div", { className: "absolute inset-0 bg-black/40 flex items-center justify-center" },
                        React.createElement("span", { className: "bg-red-600 text-white px-8 py-4 rounded-3xl font-black text-2xl uppercase tracking-widest shadow-2xl rotate-[-12deg]" }, "Vendido")
                      )
                    ),
                    React.createElement("div", { className: "flex flex-col justify-center" },
                      React.createElement("span", { className: "text-xs font-black text-amber-600 uppercase tracking-widest mb-2" }, selectedProduct.category),
                      React.createElement("h1", { className: `text-4xl font-black text-gray-900 mb-4 ${selectedProduct.isSold ? 'line-through decoration-red-500' : ''}` }, selectedProduct.title),
                      React.createElement("div", { className: "text-3xl font-black text-gray-900 mb-8" }, `${selectedProduct.price.toLocaleString()} Kz`),
                      React.createElement("div", { className: "bg-white p-6 rounded-3xl border border-gray-100 mb-8" }, React.createElement("p", { className: "text-gray-600 font-medium leading-relaxed" }, selectedProduct.description)),
                      selectedProduct.sellerId === currentUser?.id ? (
                        React.createElement("button", { onClick: () => toggleSoldStatus(selectedProduct.id), className: `w-full py-5 rounded-2xl font-black text-lg shadow-xl transition-all ${selectedProduct.isSold ? 'bg-green-600 text-white' : 'bg-red-600 text-white'}` },
                          selectedProduct.isSold ? 'Disponibilizar Novamente' : 'Marcar como Vendido'
                        )
                      ) : (
                        !selectedProduct.isSold ? (
                          React.createElement("button", { onClick: () => startChat(selectedProduct.sellerId, selectedProduct.sellerName, selectedProduct.id), className: "w-full bg-amber-600 text-white py-5 rounded-2xl font-black text-lg shadow-xl shadow-amber-100" }, "Contactar Vendedor")
                        ) : (
                          React.createElement("div", { className: "w-full bg-gray-100 text-gray-400 py-5 rounded-2xl font-black text-center text-lg uppercase tracking-widest border-2 border-dashed border-gray-200" }, "Item Esgotado")
                        )
                      )
                    )
                  ),
                  React.createElement("div", { className: "border-t pt-10" },
                    React.createElement("h2", { className: "text-2xl font-black text-gray-900 mb-6" }, "Produtos Similares"),
                    isLoadingRecs && React.createElement("div", { className: "flex justify-center items-center h-48" }, React.createElement(Loader2, { className: "w-10 h-10 text-amber-500 animate-spin" })),
                    !isLoadingRecs && recommendedProducts.length > 0 && (
                      React.createElement("div", { className: "grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6" },
                        recommendedProducts.map(p => React.createElement(ProductCard, { key: p.id, product: p, isOwner: currentUser?.id === p.sellerId }))
                      )
                    ),
                    !isLoadingRecs && recommendedProducts.length === 0 && (
                      React.createElement("p", { className: "text-center text-gray-500 py-10" }, "Nenhum produto similar encontrado.")
                    )
                  )
                ),
                view === 'profile' && currentUser && React.createElement("div", { className: "max-w-3xl mx-auto" },
                  profileSubView === 'info' ? (
                    React.createElement("div", { className: "space-y-8 animate-in fade-in" },
                      isEditingProfile ? (
                        React.createElement("form", { onSubmit: handleSaveProfile, className: "bg-white p-10 rounded-[40px] border border-gray-100 shadow-xl space-y-6" },
                          React.createElement("h2", { className: "text-2xl font-black text-gray-900 tracking-tighter uppercase mb-6" }, "Editar Meu Perfil"),
                          React.createElement("div", { className: "grid grid-cols-1 md:grid-cols-2 gap-6" },
                            React.createElement("div", { className: "space-y-1" },
                              React.createElement("label", { className: "text-xs font-black uppercase text-gray-400 ml-1" }, "Nome Completo"),
                              React.createElement("input", { name: "name", defaultValue: currentUser.name, required: true, className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none" })
                            ),
                            React.createElement("div", { className: "space-y-1" },
                              React.createElement("label", { className: "text-xs font-black uppercase text-gray-400 ml-1" }, "Nome de Usuário"),
                              React.createElement("input", { name: "username", defaultValue: currentUser.username, required: true, className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none" })
                            ),
                            React.createElement("div", { className: "space-y-1" },
                              React.createElement("label", { className: "text-xs font-black uppercase text-gray-400 ml-1" }, "Número de Telemóvel"),
                              React.createElement("input", { name: "phone", type: "tel", defaultValue: currentUser.phone, placeholder: "9xx xxx xxx", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none" })
                            )
                          ),
                          React.createElement("div", { className: "flex gap-4 pt-4" },
                            React.createElement("button", { type: "submit", className: "flex-1 bg-amber-600 text-white py-4 rounded-2xl font-black shadow-lg" }, "Salvar Perfil"),
                            React.createElement("button", { type: "button", onClick: () => setIsEditingProfile(false), className: "px-8 py-4 bg-gray-100 text-gray-500 rounded-2xl font-black" }, "Cancelar")
                          )
                        )
                      ) : (
                        React.createElement("div", { className: "bg-white p-10 rounded-[40px] border border-gray-100 shadow-xl flex items-center space-x-8" },
                          React.createElement("div", { className: "w-32 h-32 rounded-[32px] bg-amber-600 flex items-center justify-center text-white text-5xl font-black shadow-2xl shrink-0" }, currentUser.name[0].toUpperCase()),
                          React.createElement("div", { className: "flex-1" },
                            React.createElement("h2", { className: "text-3xl font-black text-gray-900 tracking-tight" }, currentUser.name),
                            React.createElement("p", { className: "text-gray-400 font-bold mb-1" }, `@${currentUser.username}`),
                            React.createElement("div", { className: "flex items-center text-gray-500 mb-4 font-bold text-sm" }, React.createElement(Phone, { className: "w-4 h-4 mr-2 text-amber-600" }), currentUser.phone || 'Sem telemóvel registado'),
                            React.createElement("div", { className: "flex gap-3" },
                              React.createElement("span", { className: `px-4 py-1.5 rounded-full text-xs font-black uppercase ${currentUser.hasPaidSubscription ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-500'}` }, currentUser.hasPaidSubscription ? 'Premium Ativo' : 'Plano Grátis'),
                              React.createElement("button", { onClick: () => setIsEditingProfile(true), className: "flex items-center text-amber-600 font-black text-xs uppercase hover:underline" }, React.createElement(Edit3, { className: "w-4 h-4 mr-1.5" }), " Editar Dados")
                            )
                          )
                        )
                      ),
                      React.createElement("div", { className: "grid grid-cols-1 md:grid-cols-3 gap-4" },
                        React.createElement("button", { onClick: () => setProfileSubView('my-ads'), className: "bg-white p-6 rounded-3xl border border-gray-100 shadow-sm flex flex-col items-center hover:border-amber-500 transition-all" }, React.createElement(Package, { className: "w-8 h-8 text-amber-600 mb-2" }), React.createElement("span", { className: "font-black text-gray-900" }, "Meus Anúncios")),
                        React.createElement("button", { onClick: () => setProfileSubView('my-purchases'), className: "bg-white p-6 rounded-3xl border border-gray-100 shadow-sm flex flex-col items-center hover:border-blue-500 transition-all" }, React.createElement(ShoppingCart, { className: "w-8 h-8 text-blue-600 mb-2" }), React.createElement("span", { className: "font-black text-gray-900" }, "Compras")),
                        React.createElement("button", { onClick: () => setProfileSubView('favorites'), className: "bg-white p-6 rounded-3xl border border-gray-100 shadow-sm flex flex-col items-center hover:border-red-500 transition-all" }, React.createElement(Heart, { className: "w-8 h-8 text-red-600 mb-2" }), React.createElement("span", { className: "font-black text-gray-900" }, "Favoritos"))
                      ),
                      React.createElement("button", { onClick: () => { setCurrentUser(null); localStorage.removeItem('carinhenga_user'); setView('home'); }, className: "w-full py-4 text-red-500 font-black flex items-center justify-center bg-red-50 rounded-2xl hover:bg-red-100 transition" }, React.createElement(LogOut, { className: "w-5 h-5 mr-2" }), " Terminar Sessão")
                    )
                  ) : (
                    React.createElement("div", { className: "animate-in slide-in-from-right" },
                      React.createElement("button", { onClick: () => setProfileSubView('info'), className: "flex items-center text-amber-600 font-black mb-6" }, React.createElement(ArrowLeft, { className: "w-5 h-5 mr-2" }), " Voltar"),
                      React.createElement("div", { className: "grid grid-cols-1 sm:grid-cols-2 gap-6" },
                        products.filter(p => {
                          if (profileSubView === 'my-ads') return p.sellerId === currentUser.id;
                          if (profileSubView === 'favorites') return currentUser.favoriteProductIds?.includes(p.id);
                          if (profileSubView === 'my-purchases') return currentUser.purchasedProductIds?.includes(p.id);
                          return false;
                        }).map(p => React.createElement(ProductCard, { key: p.id, product: p, isOwner: p.sellerId === currentUser.id }))
                      )
                    )
                  )
                ),
                view === 'add-product' && React.createElement("div", { className: "max-w-xl mx-auto bg-white p-10 rounded-[40px] border border-gray-100 shadow-2xl animate-in fade-in" },
                  React.createElement("h2", { className: "text-2xl font-black text-gray-900 mb-8 uppercase tracking-tighter" }, "O que você está vendendo?"),
                  React.createElement("form", { onSubmit: handleAddProduct, className: "space-y-6" },
                    React.createElement("input", { name: "title", required: true, placeholder: "Título do Produto", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none focus:ring-2 focus:ring-amber-500" }),
                    React.createElement("div", { className: "grid grid-cols-2 gap-4" },
                      React.createElement("input", { name: "price", type: "number", required: true, placeholder: "Preço (Kz)", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none focus:ring-2 focus:ring-amber-500" }),
                      React.createElement("select", { name: "category", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none focus:ring-2 focus:ring-amber-500" },
                        CATEGORIES.map(c => React.createElement("option", { key: c, value: c }, c))
                      )
                    ),
                    React.createElement("div", { className: "relative" },
                      React.createElement("textarea", { name: "description", rows: 4, required: true, placeholder: "Descreva o estado do produto...", value: newProductDescription, onChange: (e) => setNewProductDescription(e.target.value), className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-medium outline-none resize-none focus:ring-2 focus:ring-amber-500" }),
                      React.createElement("button", { type: "button", onClick: handleSuggestDescription, disabled: isSuggestingDescription, className: "absolute bottom-3 right-3 bg-amber-100 text-amber-600 p-2 rounded-full hover:bg-amber-200 transition disabled:opacity-50" },
                        isSuggestingDescription ? React.createElement(Loader2, { className: "w-5 h-5 animate-spin" }) : React.createElement(Sparkles, { className: "w-5 h-5" })
                      )
                    ),
                    React.createElement("button", { type: "submit", className: "w-full bg-amber-600 text-white py-5 rounded-2xl font-black hover:bg-amber-700 transition shadow-xl uppercase tracking-widest" }, "Publicar Anúncio")
                  )
                ),
                view === 'add-wanted' && React.createElement("div", { className: "max-w-xl mx-auto bg-white p-10 rounded-[40px] border border-gray-100 shadow-2xl" },
                  React.createElement("h2", { className: "text-2xl font-black text-gray-900 mb-8 uppercase tracking-tighter" }, "O que você está procurando?"),
                  React.createElement("form", { onSubmit: handleAddWanted, className: "space-y-6" },
                    React.createElement("input", { name: "title", required: true, placeholder: "O que deseja comprar?", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none focus:ring-2 focus:ring-amber-500" }),
                    React.createElement("select", { name: "category", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-bold outline-none focus:ring-2 focus:ring-amber-500" },
                      CATEGORIES.map(c => React.createElement("option", { key: c, value: c }, c))
                    ),
                    React.createElement("textarea", { name: "description", rows: 4, required: true, placeholder: "Dê detalhes (ex: marca, tamanho, cor)...", className: "w-full px-6 py-4 bg-gray-50 rounded-2xl border-none font-medium outline-none resize-none focus:ring-2 focus:ring-amber-500" }),
                    React.createElement("button", { type: "submit", className: "w-full bg-amber-600 text-white py-5 rounded-2xl font-black hover:bg-amber-700 transition shadow-xl uppercase tracking-widest" }, "Publicar Pedido")
                  )
                ),
                view === 'chats' && currentUser && React.createElement("div", { className: "grid grid-cols-1 lg:grid-cols-3 gap-10" },
                  React.createElement("div", { className: "lg:col-span-1 bg-white rounded-[40px] border border-gray-100 shadow-xl h-[600px] overflow-hidden flex flex-col" },
                    React.createElement("div", { className: "p-8 border-b bg-amber-50" }, React.createElement("h2", { className: "text-2xl font-black text-gray-900 tracking-tighter" }, "Conversas")),
                    React.createElement("div", { className: "flex-1 overflow-y-auto" },
                      chats.map(chat => (
                        React.createElement("div", { key: chat.id, onClick: () => setActiveChatId(chat.id), className: `p-6 border-b border-gray-50 cursor-pointer transition-all ${activeChatId === chat.id ? 'bg-amber-50 border-l-8 border-l-amber-600' : 'hover:bg-gray-50'}` },
                          React.createElement("p", { className: "font-black text-gray-900" }, `Negociação de ${products.find(p => p.id === chat.productId)?.title || "Produto"}`),
                          React.createElement("p", { className: "text-[10px] font-bold text-gray-400 uppercase tracking-widest" }, chat.intermediaryId ? 'Intermediado' : 'Venda Direta')
                        )
                      ))
                    )
                  ),
                  React.createElement("div", { className: "lg:col-span-2" },
                    activeChatId ? (
                      React.createElement(ChatWindow, {
                        chat: chats.find(c => c.id === activeChatId), currentUser: currentUser,
                        messages: messages.filter(m => m.productId === chats.find(c => c.id === activeChatId)?.productId),
                        products: products, onSendMessage: (text) => {
                          const active = chats.find(c => c.id === activeChatId);
                          if (active) setMessages([...messages, { id: Date.now().toString(), senderId: currentUser.id, receiverId: active.participants[0], text, timestamp: Date.now(), status: 'sent', productId: active.productId }]);
                        }
                      })
                    ) : React.createElement("div", { className: "h-[600px] bg-white rounded-[40px] border-4 border-dashed border-gray-100 flex items-center justify-center text-gray-300 font-black" }, "Selecione um chat para começar")
                  )
                ),
                view === 'auth' && React.createElement("div", { className: "max-w-md mx-auto bg-white p-12 rounded-[48px] border border-gray-100 shadow-2xl mt-12 animate-in slide-in-from-bottom-8" },
                  React.createElement("div", { className: "text-center mb-8" },
                    React.createElement("div", { className: "bg-amber-100 w-20 h-20 rounded-[32px] flex items-center justify-center mx-auto mb-6" }, React.createElement(ShoppingBag, { className: "w-10 h-10 text-amber-600" })),
                    React.createElement("h2", { className: "text-3xl font-black text-gray-900 tracking-tighter uppercase" }, authMode === 'login' ? 'Entrar' : (authMode === 'signup' ? 'Criar Conta' : 'Recuperar'))
                  ),
                  React.createElement("form", { onSubmit: handleAuth, className: "space-y-4" },
                    authMode === 'signup' && React.createElement(React.Fragment, null,
                      React.createElement("input", { name: "name", required: true, className: "w-full bg-gray-50 px-6 py-4 rounded-2xl font-bold outline-none focus:ring-2 focus:ring-amber-500", placeholder: "Nome Completo" }),
                      React.createElement("input", { name: "username", required: true, className: "w-full bg-gray-50 px-6 py-4 rounded-2xl font-bold outline-none focus:ring-2 focus:ring-amber-500", placeholder: "Nome de Usuário" })
                    ),
                    React.createElement("input", { name: "email", type: "email", required: true, onChange: () => setAuthError(null), className: "w-full bg-gray-50 px-6 py-4 rounded-2xl font-bold outline-none focus:ring-2 focus:ring-amber-500", placeholder: "E-mail" }),
                    authMode !== 'forgot' && React.createElement("input", { name: "password", type: "password", required: true, onChange: () => setAuthError(null), className: "w-full bg-gray-50 px-6 py-4 rounded-2xl font-bold outline-none focus:ring-2 focus:ring-amber-500", placeholder: "Senha" }),
                    authError && React.createElement("div", { className: "bg-red-50 border border-red-200 text-red-700 px-4 py-3 rounded-2xl text-sm font-bold flex items-center gap-2 animate-in fade-in" },
                      React.createElement(AlertCircle, { className: "w-5 h-5" }),
                      authError
                    ),
                    React.createElement("button", { type: "submit", className: "w-full bg-amber-600 text-white py-5 rounded-2xl font-black hover:bg-amber-700 transition shadow-xl mt-4 uppercase tracking-widest" }, authMode === 'login' ? 'Entrar' : 'Continuar'),
                    React.createElement("div", { className: "text-center mt-6" },
                      React.createElement("button", { type: "button", onClick: () => { setAuthMode(authMode === 'signup' ? 'login' : 'signup'); setAuthError(null); }, className: "text-sm font-black text-gray-500 hover:text-amber-600" }, authMode === 'signup' ? 'Já tem conta? Entrar' : 'Não tem conta? Cadastrar'),
                      authMode === 'login' && React.createElement("button", { type: "button", onClick: () => { setAuthMode('forgot'); setAuthError(null); }, className: "block w-full mt-2 text-xs font-black text-amber-600 uppercase" }, "Esqueci minha senha")
                    )
                  )
                )
              )
            ),
            React.createElement(Footer, null),
            React.createElement("div", { className: "md:hidden sticky bottom-0 left-0 right-0 bg-white border-t flex justify-around p-4 z-40 rounded-t-[32px] shadow-2xl" },
              React.createElement("button", { onClick: () => setView('home'), className: `flex flex-col items-center ${view === 'home' ? 'text-amber-600' : 'text-gray-400'}` }, React.createElement(ShoppingBag, { className: "w-6 h-6" }), React.createElement("span", { className: "text-[9px] mt-1 font-black uppercase" }, "Loja")),
              React.createElement("button", { onClick: () => { if (!currentUser) setView('auth'); else setView('add-product'); }, className: "flex flex-col items-center -mt-10" }, React.createElement("div", { className: "bg-amber-600 p-4 rounded-[20px] shadow-xl text-white" }, React.createElement(PlusCircle, { className: "w-6 h-6" })), React.createElement("span", { className: "text-[9px] mt-1 font-black text-amber-600 uppercase" }, "Anunciar")),
              React.createElement("button", { onClick: () => { if (!currentUser) { setView('auth'); setAuthMode('login'); } else setView('chats'); }, className: `flex flex-col items-center ${view === 'chats' ? 'text-amber-600' : 'text-gray-400'}` }, React.createElement(MessageSquare, { className: "w-6 h-6" }), React.createElement("span", { className: "text-[9px] mt-1 font-black uppercase" }, "Mensagens")),
              React.createElement("button", { onClick: () => { if (!currentUser) { setView('auth'); setAuthMode('login'); } else { setView('profile'); setProfileSubView('info'); } }, className: `flex flex-col items-center ${view === 'profile' ? 'text-amber-600' : 'text-gray-400'}` }, React.createElement(UserIcon, { className: "w-6 h-6" }), React.createElement("span", { className: "text-[9px] mt-1 font-black uppercase" }, "Perfil"))
            )
          )
        );
      }
      // --- FIM: Componente Principal da Aplicação (App.tsx) ---


      // --- INÍCIO: Lógica de Renderização (index.tsx) ---
      const rootElement = document.getElementById('root');
      if (!rootElement) {
        throw new Error("Could not find root element to mount to");
      }
      const root = ReactDOM.createRoot(rootElement);
      root.render(React.createElement(React.StrictMode, null, React.createElement(App, null)));
      // --- FIM: Lógica de Renderização (index.tsx) ---
    </script>
    <script>
      if ('serviceWorker' in navigator) {
        window.addEventListener('load', () => {
          navigator.serviceWorker.register('/service-worker.js').then(registration => {
            console.log('SW registered: ', registration);
          }).catch(registrationError => {
            console.log('SW registration failed: ', registrationError);
          });
        });
      }
    </script>
  </body>
</html>

<!-- ====================================================================== -->
<!-- ======================== FIM DE: index.html ========================== -->
<!-- ====================================================================== -->
