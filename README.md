<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Parrilladas a su Gusto - Menú Digital</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Fuentes Personalizadas -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Roboto:wght@400;700&display=swap" rel="stylesheet">

    <!-- React y Babel para compilar JSX en el navegador -->
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body class="bg-black">
    <div id="root"></div>

    <script type="text/babel">
        // --- ÍCONOS PARA LA UI ---
        const EyeIcon = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7Z"/><circle cx="12" cy="12" r="3"/></svg>;
        const SettingsIcon = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 0 2l-.15.08a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l-.22-.38a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1 0-2l.15.08a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"/><circle cx="12" cy="12" r="3"/></svg>;
        const PlusIcon = () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M5 12h14"/><path d="M12 5v14"/></svg>;
        const TrashIcon = () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M3 6h18"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>;
        const UploadIcon = () => <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" x2="12" y1="3" y2="15"/></svg>;
        const LogoutIcon = () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" x2="9" y1="12" y2="12"/></svg>;

        const { useState, useEffect, useRef } = React;

        // --- DATOS INICIALES ---
        const initialMenuData = {
          restaurantName: "Parrilladas a su Gusto",
          logoUrl: "https://placehold.co/100x100/e11d48/FFFFFF?text=PSG",
          categories: [
            { id: "cat1", name: "Nuestras Parrilladas", items: [ { id: "item1", name: "Parrillada Especial", description: "Jugosa selección de lomo, costillas, chorizo y morcilla. Ideal para 2.", price: 25.50, imageUrl: "https://placehold.co/600x400/ef4444/FFFFFF?text=Parrillada+Completa" }, { id: "item2", name: "Costillas BBQ de la Casa", description: "Tiernas costillas de cerdo bañadas en nuestra salsa BBQ secreta, a fuego lento.", price: 15.00, imageUrl: "https://placehold.co/600x400/f97316/FFFFFF?text=Costillas+BBQ" } ] },
            { id: "cat2", name: "Guarniciones", items: [ { id: "item3", name: "Papas Fritas Rústicas", description: "Papas crujientes con cáscara y un toque de paprika.", price: 3.50, imageUrl: "https://placehold.co/600x400/fbbf24/FFFFFF?text=Papas+Rústicas" }, { id: "item4", name: "Ensalada Fresca del Huerto", description: "Mix de lechugas, tomate cherry, pepino y aderezo de la casa.", price: 4.00, imageUrl: "https://placehold.co/600x400/22c55e/FFFFFF?text=Ensalada" } ] },
            { id: "cat3", name: "Bebidas", items: [ { id: "item5", name: "Jarra de Limonada Frappé", description: "Refrescante y granizada, perfecta para acompañar la parrilla.", price: 5.00, imageUrl: "https://placehold.co/600x400/3b82f6/FFFFFF?text=Limonada" }, { id: "item6", name: "Cerveza Artesanal Local", description: "Pregunta por nuestra selección del mes.", price: 4.50, imageUrl: "https://placehold.co/600x400/f59e0b/FFFFFF?text=Cerveza" } ] },
          ],
        };

        // --- COMPONENTES ---

        const BonfireEffect = () => (
            <>
                <style>{`
                    .bonfire-container { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; overflow: hidden; }
                    .ember { position: absolute; bottom: -80px; left: 50%; width: 200%; height: 150px; transform: translateX(-50%); background: radial-gradient(ellipse at center, rgba(255, 100, 0, 0.6) 0%, rgba(255, 100, 0, 0) 70%); animation: pulse 4s infinite ease-in-out; }
                    @keyframes pulse { 0% { transform: translateX(-50%) scale(1); opacity: 0.7; } 50% { transform: translateX(-50%) scale(1.1); opacity: 1; } 100% { transform: translateX(-50%) scale(1); opacity: 0.7; } }
                    .sparks { position: absolute; bottom: 0; left: 0; width: 100%; height: 100%; }
                    .sparks .spark { position: absolute; bottom: 0; list-style: none; width: 15px; height: 15px; background: #ffcc00; animation: rise 3s infinite; opacity: 0; box-shadow: 0 0 10px #ffcc00, 0 0 20px #ffcc00; clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%); }
                    .sparks .spark:nth-child(1) { left: 10%; animation-delay: 0.5s; animation-duration: 4s; }
                    .sparks .spark:nth-child(2) { left: 20%; animation-delay: 1s; animation-duration: 3s; }
                    .sparks .spark:nth-child(3) { left: 30%; animation-delay: 0s; animation-duration: 5s; }
                    .sparks .spark:nth-child(4) { left: 40%; animation-delay: 2.5s; animation-duration: 3.5s; }
                    .sparks .spark:nth-child(5) { left: 50%; animation-delay: 1.5s; animation-duration: 4.5s; }
                    .sparks .spark:nth-child(6) { left: 60%; animation-delay: 3s; animation-duration: 2.5s; }
                    .sparks .spark:nth-child(7) { left: 70%; animation-delay: 0.2s; animation-duration: 5.5s; }
                    .sparks .spark:nth-child(8) { left: 80%; animation-delay: 2s; animation-duration: 3s; }
                    .sparks .spark:nth-child(9) { left: 90%; animation-delay: 1.2s; animation-duration: 4s; }
                    .sparks .spark:nth-child(10) { left: 15%; animation-delay: 3.5s; animation-duration: 6s; }
                    @keyframes rise { 0% { transform: translateY(0) scale(1); opacity: 1; } 80% { opacity: 1; } 100% { transform: translateY(-80vh) scale(0.5); opacity: 0; } }
                `}</style>
                <div className="bonfire-container">
                    <div className="ember"></div>
                    <ul className="sparks">
                        {[...Array(10)].map((_, i) => <li key={i} className="spark"></li>)}
                    </ul>
                </div>
            </>
        );

        const CustomerMenu = ({ menuData, onNavigate }) => {
          const [activeCategory, setActiveCategory] = useState(menuData?.categories?.[0]?.id);
          useEffect(() => { if(menuData?.categories?.length > 0 && !activeCategory) { setActiveCategory(menuData.categories[0].id); } }, [menuData]);
          if (!menuData) { return <div className="text-white text-center p-10">Cargando menú...</div>; }
          const currentCategory = menuData.categories.find(c => c.id === activeCategory);
          const commonTabClass = "px-4 py-2 text-xl font-bold rounded-full transition-all duration-300 cursor-pointer bg-black/30 backdrop-blur-sm border border-white/10";
          const activeTabClass = "bg-orange-500 text-white shadow-lg border-orange-400";
          const inactiveTabClass = "text-gray-300 hover:bg-gray-700/50";
          return (
            <div className="w-full max-w-md mx-auto bg-gray-900 overflow-hidden rounded-3xl shadow-2xl shadow-black/50 border border-gray-700 relative">
              <BonfireEffect />
              <div className="relative z-10">
                <header className="pt-8 pb-6 px-6 text-center">
                  <img src={menuData.logoUrl} alt="Logo del Restaurante" className="w-28 h-28 rounded-full mx-auto mb-4 border-4 border-orange-500 shadow-lg shadow-orange-500/40" />
                  <h1 className="text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-300 via-yellow-100 to-yellow-300" style={{fontFamily: "'Playfair Display', serif", textShadow: '2px 2px 4px rgba(0,0,0,0.5)'}}>{menuData.restaurantName}</h1>
                </header>
                <nav className="p-4">
                    <div className="flex flex-wrap justify-center items-center gap-2">
                        {menuData.categories.map(category => (<button key={category.id} onClick={() => setActiveCategory(category.id)} style={{fontFamily: "'Playfair Display', serif"}} className={`${commonTabClass} ${activeCategory === category.id ? activeTabClass : inactiveTabClass}`}>{category.name}</button>))}
                    </div>
                </nav>
                <main className="p-4 sm:p-5">{currentCategory && (<div className="space-y-6">{currentCategory.items.map(item => (
                    <div key={item.id} className="transition-all duration-300 hover:scale-[1.03] rounded-2xl overflow-hidden">
                        <img src={item.imageUrl} alt={item.name} className="w-full h-48 object-cover rounded-t-2xl shadow-lg" />
                        <div className="p-5 bg-black/30 backdrop-blur-sm border-t border-white/10 rounded-b-2xl" style={{textShadow: '1px 1px 3px rgba(0,0,0,0.7)'}}>
                            <h3 className="text-3xl font-bold text-white mb-2" style={{fontFamily: "'Playfair Display', serif"}}>{item.name}</h3>
                            <p className="text-md text-gray-300 mb-4" style={{fontFamily: "'Roboto', sans-serif"}}>{item.description}</p>
                            <p className="text-3xl font-bold text-yellow-400 text-right" style={{fontFamily: "'Roboto', sans-serif"}}>${item.price.toFixed(2)}</p>
                        </div>
                    </div>
                ))}</div>)}</main>
                 <footer className="text-center p-4">
                    <button onClick={() => onNavigate('/admin')} className="text-xs text-gray-600 hover:text-gray-400">Admin Panel</button>
                </footer>
              </div>
            </div>
          );
        };

        const AdminDashboard = ({ menuData, onUpdate, onLogout }) => {
          const [localMenuData, setLocalMenuData] = useState(menuData);
          useEffect(() => { setLocalMenuData(menuData); }, [menuData]);
          const handleItemChange = (categoryId, itemId, field, value) => { const updatedData = JSON.parse(JSON.stringify(localMenuData)); const category = updatedData.categories.find(c => c.id === categoryId); const item = category.items.find(i => i.id === itemId); item[field] = value; setLocalMenuData(updatedData); };
          const handleImageUpload = (e, categoryId, itemId) => { const file = e.target.files[0]; if (file) { const reader = new FileReader(); reader.onloadend = () => { handleItemChange(categoryId, itemId, 'imageUrl', reader.result); }; reader.readAsDataURL(file); } };
          const handleAddItem = (categoryId) => { const newItem = { id: `item-${Date.now()}`, name: "Nuevo Plato", description: "Descripción del nuevo plato.", price: 0.00, imageUrl: "https://placehold.co/600x400/cccccc/FFFFFF?text=Subir+Imagen" }; const updatedData = JSON.parse(JSON.stringify(localMenuData)); const category = updatedData.categories.find(c => c.id === categoryId); category.items.push(newItem); setLocalMenuData(updatedData); };
          const handleDeleteItem = (categoryId, itemId) => { const updatedData = JSON.parse(JSON.stringify(localMenuData)); const category = updatedData.categories.find(c => c.id === categoryId); category.items = category.items.filter(item => item.id !== itemId); setLocalMenuData(updatedData); };
          const handleSaveChanges = () => { onUpdate(localMenuData); alert("¡Cambios guardados en la base de datos!"); };
          if (!localMenuData) { return <div className="text-white text-center p-10">Cargando datos...</div>; }
          return (
            <div className="w-full max-w-4xl mx-auto bg-gray-800 text-white rounded-2xl shadow-lg p-6 sm:p-8 font-sans">
               <header className="flex justify-between items-center mb-8">
                <div><h1 className="text-3xl font-bold text-white">Panel de Control</h1><p className="text-lg text-gray-400">{localMenuData.restaurantName}</p></div>
                <div><button onClick={handleSaveChanges} className="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg mr-4">Guardar Cambios</button><button onClick={onLogout} className="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-lg"><LogoutIcon/></button></div>
              </header>
              <main>
                {localMenuData.categories.map(category => (
                  <div key={category.id} className="mb-8 bg-gray-900 p-4 rounded-lg">
                    <h2 className="text-2xl font-bold text-orange-400 mb-4">{category.name}</h2>
                    <div className="space-y-4">
                      {category.items.map(item => (
                        <div key={item.id} className="flex flex-col sm:flex-row items-start p-4 bg-gray-700 rounded-lg">
                          <div className="flex-shrink-0 mr-4 mb-4 sm:mb-0 text-center">
                            <img src={item.imageUrl} alt={item.name} className="w-28 h-28 rounded-md object-cover shadow-md" onError={(e) => {e.target.onerror = null; e.target.src='https://placehold.co/200x200/000000/FFFFFF?text=Error'}}/>
                            <input type="file" id={`file-upload-${item.id}`} className="hidden" accept="image/*" onChange={(e) => handleImageUpload(e, category.id, item.id)}/>
                            <label htmlFor={`file-upload-${item.id}`} className="mt-2 inline-flex items-center justify-center px-3 py-1 bg-blue-600 hover:bg-blue-700 text-white text-xs font-bold rounded-md cursor-pointer transition-colors"><UploadIcon /><span className="ml-2">Cambiar Imagen</span></label>
                          </div>
                          <div className="flex-grow">
                            <input type="text" value={item.name} onChange={(e) => handleItemChange(category.id, item.id, 'name', e.target.value)} className="bg-transparent text-lg font-semibold w-full focus:bg-gray-600 rounded px-2 py-1 outline-none"/>
                            <textarea value={item.description} onChange={(e) => handleItemChange(category.id, item.id, 'description', e.target.value)} className="bg-transparent text-sm text-gray-300 w-full focus:bg-gray-600 rounded px-2 py-1 outline-none mt-1 h-16 resize-none"/>
                          </div>
                          <div className="flex items-center space-x-2 mt-4 sm:mt-0 sm:ml-4 self-center sm:self-auto">
                            <span className="text-lg text-gray-400">$</span>
                            <input type="number" value={item.price} onChange={(e) => handleItemChange(category.id, item.id, 'price', parseFloat(e.target.value) || 0)} className="w-24 bg-gray-600 text-white font-bold text-lg p-2 rounded-md text-center outline-none focus:ring-2 focus:ring-orange-500" step="0.01"/>
                            <button onClick={() => handleDeleteItem(category.id, item.id)} className="p-2 bg-red-600 hover:bg-red-700 rounded-md transition-colors"><TrashIcon /></button>
                          </div>
                        </div>
                      ))}
                       <button onClick={() => handleAddItem(category.id)} className="w-full mt-2 p-2 bg-green-600 hover:bg-green-700 rounded-md transition-colors flex items-center justify-center space-x-2"><PlusIcon /> <span>Añadir Plato</span></button>
                    </div>
                  </div>
                ))}
              </main>
            </div>
          );
        };

        const LoginPage = ({ onLogin, onRegister, error }) => {
            const [email, setEmail] = useState('');
            const [password, setPassword] = useState('');
            return (
                <div className="flex items-center justify-center min-h-screen bg-gray-900">
                    <div className="w-full max-w-md p-8 space-y-8 bg-gray-800 rounded-2xl shadow-lg">
                        <h2 className="text-3xl font-bold text-center text-white">Acceso al Panel</h2>
                        <div className="space-y-6">
                            <div><label className="text-sm font-bold text-gray-400">Correo Electrónico</label><input type="email" value={email} onChange={(e) => setEmail(e.target.value)} className="w-full p-3 mt-2 text-white bg-gray-700 rounded-md outline-none focus:ring-2 focus:ring-orange-500"/></div>
                            <div><label className="text-sm font-bold text-gray-400">Contraseña</label><input type="password" value={password} onChange={(e) => setPassword(e.target.value)} className="w-full p-3 mt-2 text-white bg-gray-700 rounded-md outline-none focus:ring-2 focus:ring-orange-500"/></div>
                             {error && <p className="text-red-500 text-sm text-center">{error}</p>}
                            <div className="flex flex-col space-y-4">
                                <button onClick={() => onLogin(email, password)} className="w-full py-3 font-bold text-white bg-orange-600 rounded-md hover:bg-orange-700 transition-colors">Iniciar Sesión</button>
                                <button onClick={() => onRegister(email, password)} className="w-full py-3 font-bold text-gray-300 bg-gray-700 rounded-md hover:bg-gray-600 transition-colors">Registrar (Primer Uso)</button>
                            </div>
                        </div>
                    </div>
                </div>
            );
        }

        // --- COMPONENTE PRINCIPAL DE LA APLICACIÓN ---
        function App() {
            const [route, setRoute] = useState('/');
            const [user, setUser] = useState(null);
            const [authError, setAuthError] = useState('');
            const [menuData, setMenuData] = useState(null);
            const db = useRef({ menu: null });
            useEffect(() => { if (!db.current.menu) { db.current.menu = initialMenuData; setMenuData(initialMenuData); } else { setMenuData(db.current.menu); } }, []);
            const onNavigate = (path) => { setRoute(path); };
            const handleLogin = (email, password) => { if (email && password) { setUser({ email }); setAuthError(''); onNavigate('/admin'); } else { setAuthError('Por favor, ingrese correo y contraseña.'); } };
            const handleRegister = (email, password) => { if (email && password) { setUser({ email }); setAuthError(''); onNavigate('/admin'); } else { setAuthError('Por favor, ingrese correo y contraseña para registrar.'); } };
            const handleLogout = () => { setUser(null); onNavigate('/'); };
            const handleUpdateMenu = (newData) => { db.current.menu = newData; setMenuData(newData); };
            if (route === '/admin') { if (user) { return <AdminDashboard menuData={menuData} onUpdate={handleUpdateMenu} onLogout={handleLogout} />; } else { return <LoginPage onLogin={handleLogin} onRegister={handleRegister} error={authError} />; } }
            return <CustomerMenu menuData={menuData} onNavigate={onNavigate} />;
        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);

    </script>
</body>
</html>
